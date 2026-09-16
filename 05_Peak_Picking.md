#  Peak Picking

Mass++4 picks the peaks of a spectrum or a chromatogram automatically when you
draw it. There are no settings to prepare, and the picked peaks are used for the
labels on the graphs and for saving a peak list.

This chapter assumes that an mzML file is opened, as described in
[Opening an mzML file](02_Opening_mzML.md).

##  When peaks are picked

Peaks are picked the first time you select a spectrum or a chromatogram, for
example by clicking a row of the **Spectrum** or **Chromatogram** table. The
result is kept while the file is open, so selecting the same data again does not
pick the peaks again.

##  How peaks are picked

###  Profile spectra and chromatograms

Every data point is checked as the top of a possible peak.

1. From the top, the points on both sides are followed outward.
2. If a point as high as or higher than the top is reached first, the point is
   not a peak.
3. If the intensity falls below **half of the top** on both sides, the point is
   a peak.

For each peak, the following values are recorded:

| Value | How it is calculated |
| --- | --- |
| Position (m/z or RT) | Weighted center of the points above half of the top, weighted by how far they are above it |
| Intensity | Intensity of the top |
| Start / End | The first points below half of the top, on the left and right sides |

Because the position is a weighted center, it can lie between two data points
and is more precise than the position of the highest data point.

###  Centroid spectra

A centroid spectrum already consists of peaks. Every data point with an
intensity above zero is used as a peak, with the same value for its position,
start and end.

##  Peak labels

The positions of the picked peaks are written above the peaks in the graphs: the
m/z for a spectrum and the retention time for a chromatogram, with three
decimal places.

![Spectrum with peak labels](images/screenshots/05-01-peak_labels.png)

- Labels are placed from the most intense peak downward.
- A label that would overlap a label already placed is not drawn, so only the
  major peaks are labeled in a wide range.
- Zoom in, as described in [Opening an mzML file](02_Opening_mzML.md#zooming),
  to see the labels of smaller peaks.

![Zoomed spectrum with the labels of smaller peaks](images/screenshots/05-02-peak_labels_zoom.png)

In this zoomed spectrum, the tall peak just left of `966.591` has no label,
because its label would overlap the label of `966.591`, which is placed first.

##  Saving the peak list

1. Right-click the spectrum or the chromatogram and choose **Save as** ->
   **Peak List...**.
   The item is disabled when no peaks were picked.

   ![Save as > Peak List...](images/screenshots/05-03-save_peak_list.png)

2. Choose the file name and the file type, and click `Save`.

| File name | Format |
| --- | --- |
| `*.csv` | Comma-separated values |
| `*.txt` | Tab-separated values |

If the file name has neither extension, the extension of the selected file type
is added.

The file is written in UTF-8, with the peaks sorted by position and the
following columns:

| Column | Content |
| --- | --- |
| `m/z` or `RT` | Position of the peak |
| `Intensity` | Intensity of the peak |
| `Start` | Start position of the peak |
| `End` | End position of the peak |
| `Annotation` | Annotation of the peak, if any |

Example of a CSV file:

```text
m/z,Intensity,Start,End,Annotation
100.5,300.0,100.4,100.6,
500.25,1200.0,500.1,500.4,
```
