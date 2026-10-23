# PIV-Based Cylinder Wake Analysis

## Project Overview
This project explores particle image velocimetry (PIV) processing using
Python and OpenPIV. An image pair containing a circular obstacle is analysed
to estimate particle-image displacement fields and examine their spatial
variation downstream of the obstacle.

## Tools
- Python
- NumPy
- Matplotlib
- OpenPIV
- Pandas

## Methodology
1. Load the two images as grayscale arrays.
2. Estimate displacement using OpenPIV extended-search-area PIV.
3. Apply signal-to-noise validation.
4. Investigate unusually large vectors using a local-median comparison.
5. Create a separate diagnostic field with three candidate outliers replaced
   by local-median estimates.
6. Plot horizontal and vertical displacement components.
7. Compare horizontal displacement across five neighbouring grid rows.
8. Export the downstream profile statistics to CSV.

## Reproducibility

The complete Google Colab notebook, including the analysis workflow and code, is available here:

- [Open the PIV analysis notebook](wake_analysis(1).ipynb)

The notebook uses OpenPIV to estimate image-plane displacement, identify potential outliers, and examine a preliminary downstream displacement profile.

## Preliminary PIV Results

The figure below shows the particle-image-velocimetry (PIV) displacement field after preliminary outlier correction. The vectors represent image-plane displacement in pixels per frame, not calibrated physical velocities.

![PIV displacement field after preliminary outlier correction](cleaned_piv_displacement.png)

### Output files

- [Download the downstream displacement profile (CSV)](downstream_displacement_profile(2).csv)
- [View the PIV displacement-field plot](cleaned_piv_displacement.png)
  

## Preliminary Processing Settings
- Window size: 24 pixels
- Search area size: 48 pixels
- Overlap: 12 pixels
- Correlation method: linear
- Normalized correlation: enabled
- Signal-to-noise screening threshold: 1.3

## Preliminary Observation
Across five selected grid rows, the mean horizontal image displacement
becomes more consistent at downstream sample positions. At x = 998 pixels,
the mean is approximately -2.038 pixels per image pair, with a
between-row standard deviation of approximately 0.039 pixels per image pair.

These are preliminary image-displacement statistics, not calibrated flow
velocities or proof of wake recovery.


## Output
- - [Download the downstream displacement profile](downstream_displacement_profile.csv)

## Validation and Sensitivity Analysis

### Preliminary Vector-Consistency Check

Three unusually large displacement estimates were provisionally corrected using local estimates. Their corrected values differed from the medians of their surrounding vectors by 0.074, 0.074, and 0.101 pixels per frame, respectively. This indicates local consistency after correction, but does not independently establish measurement accuracy because the corrections were derived from neighbouring vectors. The original estimates should be retained for reproducibility, and the corrected values should be treated as provisional pending independent validation.

### Signal-to-Noise Quality Check

A peak-to-peak signal-to-noise ratio (SNR) threshold of 1.3 flagged 25 of the 364 calculated displacement vectors (6.87%) for further scrutiny. The flagged vectors were spatially clustered, particularly around x = 674–818 pixels. Two vectors returned an SNR of zero and require particular caution. SNR flagging indicates potentially ambiguous correlation peaks rather than confirmed measurement errors.

### Cylinder Masking and Fluid-Region Screening

An approximate circular mask was applied around the cylinder, using an estimated centre of (720, 300) pixels and a radius of 60 pixels. Eight interrogation-window centres fell inside this mask, and all eight were flagged by the SNR criterion.

After excluding these eight centres, 17 of the remaining 356 fluid-region vectors (4.78%) were flagged for potentially ambiguous correlation peaks. These vectors were retained for further inspection rather than automatically discarded or replaced.

The mask is approximate, and classifying vectors by their centres does not fully account for interrogation windows that overlap the cylinder boundary. The flagged vectors should therefore be interpreted as candidates for further investigation, not as confirmed measurement errors.

### Interrogation-Window Sensitivity

The baseline configuration (24-pixel interrogation windows, 12-pixel overlap, and a 48-pixel search area) was compared with a finer configuration using 16-pixel windows, 8-pixel overlap, and a 32-pixel search area. Across 354 common valid locations, the median difference between displacement vectors was 0.1399 pixels per frame, while the 95th-percentile difference was 1.5157 pixels per frame.

Several localized discrepancies were substantially larger, including locations with low baseline SNR. The results indicate that the estimated displacement field is sensitive to interrogation-window settings in certain regions. This comparison is a sensitivity assessment, not independent validation of either configuration.

## Limitations and Next Steps
- Verify the dataset's provenance and the physical meaning of the image pair.
- Establish the actual frame interval and spatial calibration before
  calculating velocities in physical units.
- Investigate masked regions and correlation quality near the obstacle.
- Compare the measured field with an appropriate reference dataset or
  published benchmark before making physical claims.
 
## Project Files

- [Complete PIV analysis notebook](wake_analysis.ipynb)
- [PIV displacement-field visualization](cleaned_piv_displacement.png)
- [Downstream displacement profile (CSV)](downstream_displacement_profile.csv)
- [Python dependencies](requirements.txt)

