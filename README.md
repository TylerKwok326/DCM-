# DCM

### Data Preparation and Electrode Selection
We implemented a systematic electrode selection strategy based on task responsiveness. For each electrode, we calculated response variance across trials as a measure of task-related activity. Electrodes were ranked by variance, and the top performers were selected while ensuring spatial distribution across the OFC region. This approach identified two highly responsive electrodes (Electrodes 3 and 1) with task-related variance of 0.5775 and 0.3675, respectively.
Time series extraction created DCM-compatible data structures with dimensions: 180 trials × 3001 time points × 2 electrodes, yielding 540,180 total time points for connectivity analysis. Data quality assessment confirmed the absence of artifacts (NaN or infinite values) and appropriate signal-to-noise ratios for DCM estimation.
