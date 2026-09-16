#  Installation

Mass++4 is distributed as a ready-to-install package for each platform. The
packages contain their own Java runtime, so Java does **not** have to be
installed separately.

##  Downloading the installer

1. Open the Mass++ website: <https://mspp.ninja/>
2. Click **Downloads** in the menu at the top of the page. It opens the
   Mass++/Mass++4 portal at <https://develop.mspp.ninja/>.
3. Go to **Binary package (Installer) downloads** -> **Current (Latest)
   version**, and click the **Download page** link of the
   *Ver.4 (Mass++4) Series (Latest Build)*. It opens the
   [Mass++4 Executable File Download](https://mspp.ninja/mass4-executable-file-download/)
   page.
4. Under **Download of the installer**, find **Mass++4 ver.1.0.0** and click the
   download button of the package for your platform.

| Platform | Package on the page | Format |
| --- | --- | --- |
| Windows | `Mass++4 1.0 (Windows)` | `.zip` (zipped `.exe`) |
| macOS | `Mass++4 1.0 (MacOS)` | `.dmg` |
| Debian / Ubuntu | `Mass++4 1.0 (Linux-Debian/Ubuntu)` | `.deb` |
| RHEL / AlmaLinux and other RPM distributions | `Mass++4 1.0 (Linux-RPM)` | `.rpm` |

Older builds (ver.0.2.1 and earlier) are listed further down the same page under
*[Mass++4 Installer - Previous Versions]*. Mass++ ver.2 has its own download
page and is not covered by this manual.

The same installers are also attached to the release in the source repository:
<https://github.com/masspp/mspp4-desktop/releases>

##  Windows

1. Extract the downloaded `.zip` file.
2. Run the installer (`.exe`) that it contains.
3. The installer lets you choose the installation folder and adds a Start menu
   entry and a desktop shortcut.
4. Start the application from the Start menu entry **Mass++4**.

##  macOS

1. Open the downloaded `.dmg` file.
2. Drag **Mass++4** into the `Applications` folder.
3. Start the application from the `Applications` folder.

If macOS refuses to open the application because it is not from an identified
developer, right-click the application icon, choose `Open`, and confirm.

##  Debian / Ubuntu

Install the downloaded package:

```bash
sudo apt install ./ms++4_1.0.0_amd64.deb
```

##  RHEL / AlmaLinux

Install the downloaded package:

```bash
sudo dnf install ./ms++4-1.0.0-1.alma9.x86_64.rpm
```

##  Opening vendor raw files (optional)

Reading `.mzML` files works out of the box. Opening Thermo `.raw`, Sciex
`.wiff`, or Shimadzu `.lcd` files additionally requires **Docker Desktop** to be
installed and running, because Mass++4 converts those files to mzML with
ProteoWizard `msconvert` inside a Docker container.

<https://www.docker.com/products/docker-desktop/>

The first conversion downloads the converter image, so it takes longer than
later ones. If Docker is not running, Mass++4 shows a message asking you to
start Docker Desktop.

##  Building from source

To build Mass++4 yourself instead of using an installer, follow the
instructions in the `mspp4-desktop` repository:

<https://github.com/masspp/mspp4-desktop#how-to-develop>
