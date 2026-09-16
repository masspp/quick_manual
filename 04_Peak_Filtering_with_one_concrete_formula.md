#  Peak Filtering with One Concrete Formula

The peak filter finds the MS/MS spectra that contain a peak at a given m/z. This
chapter walks through one concrete example from the formula to the result.

**Example**: find the MS/MS spectra that contain the HexNAc oxonium ion, a
fragment ion that is typical for glycopeptides.

| Item | Value |
| --- | --- |
| Formula | `C8H13NO5` |
| Ion | `[M+H]+` |
| m/z | `204.086648963` |

This chapter assumes that an mzML file is opened and its sample is selected, as
described in [Opening an mzML file](02_Opening_mzML.md). The peak filter
searches the spectra of the selected sample.

##  Step 1: Calculate the m/z from the formula

1. Choose **Tools** -> **Mass Calculator...**.
2. Set **Type** to **Chemical composition**.
3. Enter `C8H13NO5` in **Name**.
   **Mass** shows the neutral mass, `203.079372531`.
4. Keep **[M+H]+** selected, and leave **Water loss (-H2O)** unchecked.
   **m/z** shows `204.086648963`.

![Mass Calculator with C8H13NO5](images/screenshots/04-01-mass_calculator.png)

If the formula cannot be read, a message in red is shown below **Mass** and the
values are cleared.

You get the same m/z by setting **Type** to **Glycan composition**, entering
`HexNAc(1)`, and checking **Water loss (-H2O)**.

##  Step 2: Add the m/z to the peak filter

1. In the **Mass Calculator**, click **Search peak**.
   The **Peak Filter** dialog opens with **Name** set to `C8H13NO5 + H` and
   **m/z** set to `204.086648963`.
2. Choose the color used to mark the peak with **Color**.
3. Click **Add**. The peak is added to the table at the top of the dialog, and
   the input fields are cleared for the next peak.

You can also open the dialog with **Tools** -> **Peak Filter...** and type
**Name** and **m/z** directly. To search for several ions at once, add a row for
each of them. To remove a row, select it and click **Delete**.

##  Step 3: Search

1. Enter the search conditions at the bottom of the dialog. Both fields are
   empty at first and must be filled in.

   | Field | Meaning | Example |
   | --- | --- | --- |
   | **m/z Tolerance** | Allowed difference from the m/z, in m/z | `0.01` |
   | **Intensity Threshold** | Minimum intensity of the peak | `1` with `%` |

   The unit of **Intensity Threshold** is either `%` (percent of the most
   intense peak of each spectrum) or `count` (absolute intensity).
   Choose a tolerance that matches the accuracy of your instrument.

2. Click **Search**.

![Peak Filter dialog with the peak and the search conditions](images/screenshots/04-02-peak_filter.png)

Only MS/MS spectra (MS Stage 2 or higher) are searched. A spectrum is a hit when
it has a peak within **m/z** ± **m/z Tolerance** whose intensity is at least the
threshold.

##  Step 4: Check the result

The **Peak Filter Result** dialog lists the hit spectra.

![Peak Filter Result dialog](images/screenshots/04-03-result.png)

| Column | Content |
| --- | --- |
| `Spectrum` | The spectrum |
| `RT` | Retention time |
| `MS Stage` | MS stage |
| `Precursor` | Precursor m/z |
| One column for each peak, titled with its name and m/z | Intensity of the matching peak |

Click a row to draw that spectrum. The m/z of the peak is marked with a dotted
line and the peak name in the color you chose.

![Spectrum with the matching position marked](images/screenshots/04-04-spectrum_label.png)

Uncheck **Display peak information on the spectrum** to hide the marks.

On the **Heatmap** tab, the precursor positions of the hit spectra are marked
with small squares in the same color. This shows where in the retention time
and m/z the precursors of the hit spectra are.

![Heatmap with the precursor positions of the hit spectra](images/screenshots/04-05-heatmap_marks.png)

Click **Close** to close the result.

##  Saving the peak list for later

The peaks in the **Peak Filter** dialog can be saved as a named set and reused.
The list at the top of the dialog shows the selected set, or `(New)` when no
set is selected.

- **Save As ...**: saves the peaks as a new set. Enter a name when asked.
- **Save**: overwrites the selected set. When no set is selected, it works like
  **Save As ...**.
- Select a set in the list to load its peaks. They replace the peaks in the
  table.
- **Delete...**: deletes the selected set after a confirmation.
