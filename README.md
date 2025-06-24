# DCM

### Data Preparation and Electrode Selection

  Implemented a systematic electrode selection strategy based on task responsiveness. For each electrode, response variance across trials was calculated as a measure of task-related activity. Electrodes were ranked by variance, with the top performers selected while ensuring spatial distribution across the OFC region. This approach identified two highly responsive electrodes (Electrodes 3 and 1) with task-related variance of 0.5775 and 0.3675, respectively. Time series extraction generated DCM-compatible data structures with dimensions of 180 trials × 3001 time points × 2 electrodes, resulting in 540,180 total time points for connectivity analysis. Data quality assessment confirmed the absence of artifacts (NaN or infinite values) and appropriate signal-to-noise ratios for DCM estimation.

### Experimental Input Design

- DCM requires explicit specification of experimental inputs that drive neural responses. We designed three input functions based on the gambling task structure:
  
Input 1 (Driving): Game onset events modeled as delta functions at trial initiation (750ms post-fixation), representing sensory-cognitive input that directly drives neural activity.
Input 2 (Modulatory): Choice type context, coded as sustained binary signals (1 for gamble choices, 0 for safe bet choices) lasting from stimulus presentation to choice execution, representing contextual modulation of connectivity.
Input 3 (Modulatory): Outcome valence, coded as transient signals (1 for win outcomes, 0 for loss outcomes) lasting 500ms post-outcome revelation, representing feedback-based connectivity modulation.
Input validation confirmed appropriate event counts (180 game onsets, 224,000 choice-type samples, 11,500 outcome samples) and correct temporal alignment with neural data.
