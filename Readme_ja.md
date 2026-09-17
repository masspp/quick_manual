#  Mass++4 クイックマニュアル

[English version](Readme.md)

質量分析データのオープンソースのビューア [Mass++4](https://mspp.ninja/) の
クイックマニュアルです。Mass++4 の次のリリースを対象にしています。

##  目次

| No. | 章 | 内容 |
| --- | --- | --- |
| 00 | [現在の制限事項](00_Current_Limitations_ja.md) | Mass++4 でまだできないこと |
| 01 | [インストール](01_Installation_ja.md) | Windows、macOS、Linux への Mass++4 のダウンロードとインストール |
| 02 | [mzML ファイルを開く](02_Opening_mzML_ja.md) | ファイルを開く、クロマトグラムとスペクトルの描画、ズーム、パン、ヒートマップ、3D 表示 |
| 03 | [TIC、XIC とスペクトル間の移動](03_TIC_XIC_and_Spectrum_Navigation_ja.md) | TIC の使い方、XIC の作成、スペクトル間の移動 |
| 04 | [具体的な組成式を使ったピークフィルタ](04_Peak_Filtering_with_one_concrete_formula_ja.md) | 組成式から計算したイオンを含む MS/MS スペクトルの検索 |
| 05 | [ピーク検出](05_Peak_Picking_ja.md) | ピークの検出方法、ピークラベル、ピークリストの保存 |
| 06 | [SVG の書き出し](06_SVG_Export_ja.md) | スペクトルとクロマトグラムを SVG の図として保存 |
| 07 | [Python との MS データ連携の例](07_Python_MS_data_link_example_ja.md) | API を使った Mass++4 と Python の間のスペクトルのやり取り |

Mass++4 を初めて使う場合は、まず 00〜02 の章を読んでください。ほかの章は、どの順番
で読んでもかまいません。

##  関連リンク

- Mass++ ウェブサイト：<https://mspp.ninja/>
- Mass++4 のソースコード：[mspp4-core](https://github.com/masspp/mspp4-core) と
  [mspp4-desktop](https://github.com/masspp/mspp4-desktop)
- API の例：[api-sample](https://github.com/masspp/api-sample)
