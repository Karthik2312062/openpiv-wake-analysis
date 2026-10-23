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

- [Open the PIV analysis notebook](wake_analysis.ipynb)

The notebook uses OpenPIV to estimate image-plane displacement, identify potential outliers, and examine a preliminary downstream displacement profile.

## Preliminary PIV Results

The figure below shows the particle-image-velocimetry (PIV) displacement field after preliminary outlier correction. The vectors represent image-plane displacement in pixels per frame, not calibrated physical velocities.

![PIV displacement field after preliminary outlier correction](cleaned_piv_displacement.png)

### Output files

- [Download the downstream displacement profile (CSV)](downstream_displacement_profile.csv)
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

### Preliminary Vector-Consistency Check

Three unusually large displacement estimates were provisionally corrected using local estimates. Their corrected values differed from the medians of their surrounding vectors by 0.074, 0.074, and 0.101 pixels per frame, respectively. This indicates local consistency after correction, but does not independently establish measurement accuracy because the corrections were derived from neighbouring vectors. The original estimates should be retained for reproducibility, and the corrected values should be treated as provisional pending independent validation.

### Signal-to-Noise Quality Check

A peak-to-peak signal-to-noise ratio (SNR) threshold of 1.3 flagged 25 of the 364 calculated displacement vectors (6.87%) for further scrutiny. Two vectors in the raw field were non-finite. SNR flagging indicates potentially ambiguous correlation peaks rather than confirmed measurement errors. These results provide a preliminary assessment of vector quality; they do not establish overall measurement accuracy. The threshold choice and its effect on the resulting displacement field should be evaluated in future sensitivity and validation studies.

## Limitations and Validation

This project presents a preliminary particle image velocimetry (PIV) analysis of an image pair from the OpenPIV example dataset. The results should be interpreted as image-plane displacement estimates rather than calibrated flow velocities.

- **Spatial calibration:** The physical distance represented by each pixel has not been established. Therefore, displacement values are reported in pixels per frame and cannot be converted into physical velocity units.
- **Time calibration:** The time interval between the two images has not been verified independently. Consequently, velocity in m/s cannot be calculated reliably.
- **Outlier correction:** Potentially anomalous vectors were identified and corrected using local estimates. These corrections are preliminary and have not been independently validated against ground-truth measurements.
- **Downstream profile:** The horizontal displacement profile was extracted from selected rows near the estimated cylinder centreline. Its downstream trend is exploratory and does not, by itself, establish wake recovery.
- **Measurement uncertainty:** The analysis has not yet been validated against published reference data or independent measurements. Sensitivity to interrogation-window parameters and image quality also remains to be assessed.

### Planned Validation

Future work will verify the image-pair timing and spatial scale where metadata are available, assess vector quality and sensitivity to processing parameters, and compare the resulting flow field with suitable reference data. Physical velocities and quantitative wake-recovery claims will be reported only after the necessary calibration and validation steps.

## Limitations and Next Steps
- Verify the dataset's provenance and the physical meaning of the image pair.
- Establish the actual frame interval and spatial calibration before
  calculating velocities in physical units.
- Investigate masked regions and correlation quality near the obstacle.
- Validate outlier detection and replacement against the raw measurements.
- Compare the measured field with an appropriate reference dataset or
  published benchmark before making physical claims.
 
## Project Files

- [Complete PIV analysis notebook](wake_analysis.ipynb)
- [PIV displacement-field visualization](cleaned_piv_displacement.png)
- [Downstream displacement profile (CSV)](downstream_displacement_profile.csv)
- [Python dependencies](requirements.txt)

