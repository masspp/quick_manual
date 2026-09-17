#  Python との MS データ連携の例

Mass++4 には、ほかのプログラムとスペクトルをやり取りするための小さな HTTP API が
あります。この章では、Python から両方向に使う方法を説明します。

- **Python -> Mass++4**：Python で作ったスペクトルを送り、Mass++4 で表示します。
- **Mass++4 -> Python**：Mass++4 で選択中のサンプルのスペクトルを読み込み、Python で
  処理します。

API とそのほかの例は、[api-sample](https://github.com/masspp/api-sample) リポジトリ
で公開されています。

##  必要なもの

- **Mass++4 が起動していること**：API は Mass++4 と一緒に自動で起動し、ポート
  `8191` でリクエストを待ち受けます。
- **Python 3** と、`requests` および `numpy`：

  ```bash
  pip install requests numpy
  ```

API には認証がなく、ネットワーク上のほかのコンピュータからもこのポートに接続でき
ます。信頼できるネットワークで使い、ほかのコンピュータから使う必要がなければ、
ファイアウォールでポート `8191` をブロックしてください。

##  API の仕組み

- どのサービスも `POST http://localhost:8191/<サービス名>` で呼び出します。
- リクエストの本文は JSON です。パラメータのないサービスには `null` を送ります。
- 応答は、`"{\"index\":\"0\"}"` のように JSON の文字列で包まれた JSON なので、2 回
  デコードする必要があります。応答の数値が `"0"` のように文字列になることもあります。

| サービス | パラメータ | 結果 |
| --- | --- | --- |
| `io_create_sample` | `null` | スペクトルの送り先のサンプルを作り、その `id` を返します |
| `io_add_scan` | スペクトル（下記参照） | スペクトルをサンプルに追加します |
| `io_add_annotation` | アノテーションのリスト | ピークの上に表示する構造の画像を追加します |
| `io_flush` | `id` と `index` | サンプルを Mass++4 で開き、`index` のスペクトルを描画します |
| `io_get_spectra_count` | `null` | 選択中のサンプルのスペクトルの本数 |
| `io_get_current_index` | `null` | **Spectrum** テーブルで選択しているスペクトルの番号 |
| `io_get_spectrum` | `index`（文字列） | 選択中のサンプルの `index` 番目のスペクトル |

##  呼び出し用の関数

次の内容を `masspp_api.py` として保存します。以降の例では、この関数でサービスを呼び
出します。api-sample の `common_ms_utils.py` にある `call_service` と同じ仕組みです。

```python
import json

import requests

BASE_URL = 'http://localhost:8191/'


def call_service(name, data=None):
    """Calls a Mass++4 service and returns the decoded JSON response."""
    response = requests.post(
        BASE_URL + name,
        headers={'Content-Type': 'application/json'},
        data=json.dumps(data),
    )
    response.raise_for_status()
    if not response.text.strip():
        raise RuntimeError(f'{name} returned an empty response')
    result = response.json()
    # Mass++4 returns the JSON document as a JSON string, so decode it once more.
    if isinstance(result, str):
        result = json.loads(result)
    return result
```

##  例 1：Python から Mass++4 にスペクトルを送る

この例では、2 つの化合物を含む小さな LC-MS データを作り、Mass++4 に送ります。
`masspp_api.py` と同じフォルダに `send_spectra.py` として保存します。

| 化合物 | m/z | 保持時間 |
| --- | --- | --- |
| 1 | `445.10` | 3.0 分 |
| 2 | `622.05` | 6.0 分 |

```python
import numpy as np

from masspp_api import call_service

# Two compounds: (m/z, retention time in minutes)
COMPOUNDS = [(445.10, 3.0), (622.05, 6.0)]

mz = np.round(np.arange(400.0, 700.0, 0.05), 2)
rts = np.round(np.arange(0.0, 10.0, 0.1), 1)

sample_id = call_service('io_create_sample')['id']

for rt in rts:
    intensity = np.zeros_like(mz)
    for peak_mz, peak_rt in COMPOUNDS:
        height = 1.0e6 * np.exp(-((rt - peak_rt) / 0.3) ** 2)
        intensity += height * np.exp(-((mz - peak_mz) / 0.02) ** 2)

    call_service('io_add_scan', {
        'id': sample_id,
        'msLevel': 1,
        'precursorMz': -1.0,
        'rt': float(rt),
        'centroidMode': False,
        'minMz': 400.0,
        'maxMz': 700.0,
        'points': [{'x': float(x), 'y': float(y)} for x, y in zip(mz, intensity)],
    })

# Open the sample in Mass++4 and draw the spectrum at index 30 (RT 3.0).
call_service('io_flush', {'id': sample_id, 'index': 30})
print(f'Sent {len(rts)} spectra as sample {sample_id}')
```

Mass++4 を起動した状態で実行します。

```bash
python send_spectra.py
```

100 本のスペクトルが数秒で送られます。

###  Mass++4 での表示

データは **Sample** テーブルに `External Data 1` として表示されます。番号は、データ
を送るたびに増えます。Mass++4 で最初に開いたサンプルであれば自動で選択され、
index `30`（RT 3.0）のスペクトルが、化合物 1 の m/z 445.1 のピークとともに描画され
ます。

![スペクトルを送った後の Mass++4](images/screenshots/07-01-send_result.png)

API で送ったスペクトルでは自動のピーク検出が行われないため、これらのスペクトルには
ピークラベルが表示されません。

スペクトルから TIC が計算され、**Chromatogram** テーブルに表示されます。クリックする
と、3.0 分と 6.0 分に 2 つの化合物が描画されます。

![送ったスペクトルの TIC](images/screenshots/07-02-send_tic.png)

**Heatmap** タブと **3D** タブは、開いたファイルと同じように使えます。**3D** タブ
では、2 つの化合物が 2 つのピークとして表示されます。

![送ったスペクトルの 3D 表示](images/screenshots/07-03-send_3d.png)

###  スペクトルの項目

| 項目 | 意味 |
| --- | --- |
| `id` | `io_create_sample` が返したサンプルの ID |
| `msLevel` | MS ステージ。MS は `1`、MS/MS は `2` |
| `precursorMz` | プリカーサーの m/z。MS スペクトルでは `-1.0` |
| `rt` | 保持時間（分） |
| `centroidMode` | centroid データは `true`、profile データは `false` |
| `minMz`、`maxMz` | スペクトルの m/z の範囲 |
| `points` | `{"x": m/z, "y": 強度}` のリストで表したデータ点 |

`io_flush` の `index` は、描画するスペクトルの位置で、送った順に `0` から数えます。

##  例 2：Mass++4 のスペクトルを Python に読み込む

この例では、選択中のサンプルのすべてのスペクトルを読み込み、各スペクトルのベース
ピーク（最も強いデータ点）を CSV ファイルに保存します。`read_spectra.py` として保存
します。

```python
import csv

from masspp_api import call_service

count = int(call_service('io_get_spectra_count')['count'])
print(f'The selected sample has {count} spectra')

with open('base_peaks.csv', 'w', newline='') as f:
    writer = csv.writer(f)
    writer.writerow(['index', 'MS level', 'RT', 'base peak m/z', 'base peak intensity'])
    for index in range(count):
        spectrum = call_service('io_get_spectrum', {'index': str(index)})
        points = spectrum['points']
        if not points:
            continue
        base = max(points, key=lambda p: p['y'])
        writer.writerow([index, spectrum['msLevel'], spectrum['rt'], base['x'], base['y']])

print('Saved base_peaks.csv')
```

1. Mass++4 の **Sample** テーブルでサンプルを選択します。Mass++4 で開いたファイル
   でも、例 1 で送ったデータでもかまいません。
2. スクリプトを実行します。

   ```bash
   python read_spectra.py
   ```

例 1 で送ったデータの場合、`base_peaks.csv` には 2 つの化合物がそれぞれの保持時間で
記録されます。

```text
index,MS level,RT,base peak m/z,base peak intensity
...
30,1,3.0,445.1,1000000.0
...
60,1,6.0,622.05,1000000.0
...
```

`io_get_spectrum` は、例 1 の表と同じ項目を返します。大きなファイルでは、スペクトル
1 本ごとに 1 回リクエストするため、すべてを読み込むのに時間がかかります。

##  api-sample のそのほかの例

[api-sample](https://github.com/masspp/api-sample) リポジトリには、ほかにもスクリプト
があります。必要なパッケージは `pip install -r requirements.txt` でインストールします。

| スクリプト | 内容 |
| --- | --- |
| `mzXML.py` | mzXML ファイルを pyteomics で読み込み、スペクトルを Mass++4 に送ります |
| `MoNA.py` | MoNA からスペクトルをダウンロードし、アノテーションのあるピークの構造の画像（RDKit で描画）と一緒に送ります |
| `extract.py` | 選択中のサンプルのすべてのスペクトルについて、MS レベル、RT、データ点の数を表示します |

##  トラブルシューティング

| 症状 | 原因と対処 |
| --- | --- |
| `Connection refused` | Mass++4 が起動していません。Mass++4 を起動し、ほかのプログラムがポート `8191` を使っていないか確認してください。 |
| `io_get_spectra_count returned an empty response` | サンプルが選択されていません。**Sample** テーブルでサンプルを選択してください。 |
| そのほかのサービスが空の応答を返す | パラメータが誤っています（存在しないサンプルの `id` や、範囲外の `index` など）。エラーの詳細は Mass++4 のコンソール出力に表示されます。ターミナルから Mass++4 を起動すると確認できます。 |
