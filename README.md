### 1) Data Preparation and Electrode Selection

A systematic electrode selection strategy was implemented based on task responsiveness. For each electrode, response variance across trials was calculated as a measure of task-related activity. Electrodes were ranked by variance, with the top performers selected while ensuring spatial distribution across the OFC region. This approach identified two highly responsive electrodes (Electrodes 3 and 1) with task-related variance of 0.5775 and 0.3675, respectively.

Time series extraction generated DCM-compatible data structures with dimensions of 180 trials × 3001 time points × 2 electrodes, yielding 540,180 total time points for connectivity analysis. Data quality assessment confirmed the absence of artifacts (NaN or infinite values) and appropriate signal-to-noise ratios for DCM estimation.

### 2) Experimental Input Design

DCM requires explicit specification of experimental inputs that drive neural responses. Three input functions were designed based on the gambling task structure:

- Input 1 (Driving): Game onset events modeled as delta functions at trial initiation (750ms post-fixation), representing sensory-cognitive input that directly drives neural activity.
- Input 2 (Modulatory): Choice type context, coded as sustained binary signals (1 for gamble choices, 0 for safe bet choices) lasting from stimulus presentation to choice execution, representing contextual modulation of connectivity.
- Input 3 (Modulatory): Outcome valence, coded as transient signals (1 for win outcomes, 0 for loss outcomes) lasting 500ms post-outcome revelation, representing feedback-based connectivity modulation.
  
Input validation confirmed appropriate event counts (180 game onsets, 224,000 choice-type samples, 11,500 outcome samples) and correct temporal alignment with neural data.

### 3) Network Architecture Design

Based on theoretical considerations of OFC functional organization, a 2-electrode network architecture with bidirectional connections was specified. The connectivity structure included:

- A Matrix (Intrinsic Connectivity): A 2×2 matrix encoding baseline connectivity between electrodes, with diagonal elements representing self-connections (decay rates) and off-diagonal elements representing inter-electrode coupling.
- B Matrices (Modulatory Connectivity): A 3×2×2 tensor encoding input-dependent connectivity changes, with separate matrices for each experimental input's modulatory effects.
- C Matrix (Driving Connectivity): A 2×3 matrix specifying which inputs directly drive each electrode, with the primary driving input (game onset) affecting both electrodes.
Network architecture visualization confirmed physiologically plausible connectivity patterns, with appropriate parameter constraints ensuring system stability.

### 4) Model Specification and Forward Modeling

The DCM forward model was implemented using Euler integration of the bilinear differential equation. The forward model simulated neural responses given parameter values and experimental inputs, enabling likelihood calculation for Bayesian parameter estimation.

ECoG-specific adaptations included:

- Faster temporal scaling (2.0s⁻¹ vs. 1.0s⁻¹ for fMRI) to account for direct neural measurement.
- Elimination of hemodynamic modeling components, as ECoG directly measures neural activity.
- Adjustment of prior distributions to reflect ECoG noise characteristics and physiological constraints.
Parameter constraints were strategically applied to reduce model complexity while preserving key hypotheses. A total of 9 free parameters were estimated: 4 intrinsic connectivity parameters, 3 modulatory parameters, and 2 driving parameters.

### 5) Bayesian Parameter Estimation

Parameter estimation employed maximum a posteriori (MAP) estimation using L-BFGS-B optimization. The objective function combined data likelihood (assuming Gaussian observation noise) with prior probability densities. To manage computational complexity, initial estimation used a temporally subsampled dataset (54,018 time points) to establish parameter ranges and verify algorithm stability.

Prior specifications included:

- Weakly informative priors on connectivity parameters (mean = 0, variance = 0.25).
- Stability constraints ensuring negative real eigenvalues of the A matrix.
- Parameter bounds preventing unrealistic connectivity strengths.
