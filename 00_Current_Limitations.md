#  Current Limitations

This page lists what Mass++4 cannot do yet, or does only partly. Please check it
before you start, together with the other chapters of this manual.

##  Data files

- **Supported formats**: **File** -> **Open** -> **MS Data...** opens mzML,
  Thermo `.raw`, Sciex `.wiff` and Shimadzu `.lcd` files. Other formats, such as
  mzXML, MGF or Bruker `.d`, cannot be opened directly. Convert them to mzML
  first, for example with ProteoWizard `msconvert`.
- **Vendor files need Docker**: `.raw`, `.wiff` and `.lcd` files are converted
  to mzML with ProteoWizard `msconvert` in a Docker container, so Docker Desktop
  must be installed and running. The first conversion downloads the converter
  image, which needs an internet connection and takes time. See
  [Installation](01_Installation.md#opening-vendor-raw-files-optional).
- **Conversion menu**: **File** -> **Convert** -> **mzML...** accepts only
  `.raw` and `.wiff` files. To use a `.lcd` file, open it with
  **File** -> **Open** -> **MS Data...**.
- **Opening the same file twice**: a file that is already open cannot be opened
  again.
- **Closing samples**: opened samples cannot be closed. Restart Mass++4 to clear
  them.
- **Saving data**: Mass++4 does not save or export MS data files. You can save
  images of graphs (PNG and SVG) and peak lists (CSV and tab-separated text).

##  Display

- **Heatmap and 3D view**: only MS spectra (MS Stage 1) are used, and at least
  two of them with a retention time are needed. See
  [Opening an mzML file](02_Opening_mzML.md#heatmap).
- **Image export**: PNG and SVG files can be saved from spectra and
  chromatograms only, not from the heatmap or the 3D view. See
  [SVG Export](06_SVG_Export.md).

##  Peak picking

- **No settings**: the peak picking method cannot be changed or tuned. See
  [Peak Picking](05_Peak_Picking.md).
- **Peak labels**: the positions are always written with three decimal places,
  and labels that would overlap are not drawn.
- **Spectra sent through the API**: peaks are not picked. Only the peaks sent
  with `io_add_annotation` are used for labels and peak lists, so without
  annotations no labels are drawn and no peak list can be saved.

##  XIC and averaged spectra

- **XIC**: only MS spectra are used, and the m/z range is given as an absolute
  tolerance in m/z, not in ppm. See
  [TIC, XIC and Spectrum Navigation](03_TIC_XIC_and_Spectrum_Navigation.md#xic).
- **Averaged spectra of centroid data**: only peaks at exactly the same m/z are
  combined. Peaks with slightly different m/z in different spectra are kept as
  separate peaks.

##  Peak filter and Mass Calculator

- **Search target**: the peak filter searches MS/MS spectra only. See
  [Peak Filtering with One Concrete Formula](04_Peak_Filtering_with_one_concrete_formula.md).
- **Tolerance**: the m/z tolerance is an absolute value in m/z, not in ppm.
- **Neutral Loss**: the **Neutral Loss** option is stored with the peak, but it
  is not used by the search yet.
- **Ions in the Mass Calculator**: only positive ions can be calculated:
  `[M+H]+`, `[M+2H]+`, `[M+3H]+`, `[M+Na]+`, `[M+2Na]+`, `[M+3Na]+` and
  `[M+X]+`.

##  Python API

- **No authentication**: anyone who can reach port `8191` of the computer can
  use the API, including other computers on the network. See
  [Python MS Data Link Example](07_Python_MS_data_link_example.md#requirements).
- **Fixed port**: the API always uses port `8191`.
- **Selected sample only**: `io_get_spectra_count`, `io_get_current_index` and
  `io_get_spectrum` work on the sample selected in the **Sample** table.
- **Error messages**: when a request fails, for example because no sample is
  selected, the API returns an empty response instead of an error message.
- **Annotations**: only the structure images of `io_add_annotation` are drawn.
  The `name` of an annotation is not shown.
