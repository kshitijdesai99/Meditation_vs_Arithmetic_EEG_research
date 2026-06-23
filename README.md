# Meditation vs. Arithmetic EEG Classification

This research project studies whether internally focused meditation and externally focused mental arithmetic produce measurably different EEG patterns. It combines two public EEG sources, engineers interpretable neurophysiological features, and trains a Random Forest classifier to distinguish the two cognitive states.

The emphasis is not only classification accuracy, but also explaining which brain-wave features drive the decision and how those features relate to established neuroscience.

## Final reported outcome

The final Part D analysis reports:

- **113,240 EEG records**;
- **112 participant identifiers** in the processed dataset: 72 arithmetic and 40 meditation;
- **19 common electrode positions** across the source datasets;
- **26 neurophysiological features**;
- a participant-isolated 80/20 split with 89,756 training and 23,484 test samples;
- **100% Random Forest test accuracy**.

The most important reported discriminator was delta activity:

| Feature | Random Forest importance |
| --- | ---: |
| Delta power | 28.4% |
| Delta relative power | 19.6% |
| RMS amplitude | 9.2% |
| Signal energy | 8.7% |
| Dominant frequency | 6.4% |

In the processed data, arithmetic samples had essentially zero delta power while meditation samples showed substantial delta power. The Alpha/Theta ratio also changed from approximately **0.98 for arithmetic** to **5.27 for meditation**.

These are the results of the final report and supersede the smaller preliminary experiment described in the historical `README.txt`.

## Research question

The project asks:

- Can raw EEG be converted into interpretable features grounded in neuroscience?
- Which frequency, amplitude, spectral, and complexity measures distinguish meditation from arithmetic?
- Are the differences local to particular brain regions or distributed across the brain?
- Do the signatures remain stable over time?
- Can an interpretable model support practical neurofeedback or brain-computer-interface applications?

## Data sources

### Meditation

OpenNeuro dataset `ds001787` contains long meditation recordings collected with a BioSemi EEG system. The original study includes experienced and less-experienced meditators, high-density EEG channels, and additional physiological sensors.

### Mental arithmetic

The PhysioNet EEGMAT dataset contains recordings collected while participants performed serial-subtraction tasks. It uses a different EEG system and sampling rate from the meditation data.

The raw BDF/EDF files and generated CSV feature tables are not included in this repository.

Expected raw-data folders:

```text
meditation_bdf_files/
arithmetic_eeg_files/
```

Later notebooks also expect:

```text
arithmetic_eeg_features_seconds.csv
meditation_eeg_features_minutes.csv
combined_eeg_features_complete.csv
```

## Analysis pipeline

### 1. Raw signal inspection

`a1936769_assignment1A.ipynb` loads BDF and EDF files with `pyedflib`, inspects channels and signal ranges, and compares the source datasets. Welch's method is used to estimate power spectral density and summarize standard EEG bands.

### 2. Initial feature extraction and modeling

`a1936769_assignment1B.ipynb` develops the reusable feature-extraction function and an early experiment over a smaller set of recordings. It calculates band powers, relative powers, ratios, spectral descriptors, and complexity measures before exploring:

- K-means clustering;
- PCA;
- Random Forest classification;
- statistical significance tests;
- feature importance.

This notebook represents an intermediate stage and reports preliminary results that differ from the final expanded analysis.

### 3. Expanded EDA and participant-isolated model

`a1936769_assignment1C.ipynb` contains the final large-scale analytical workflow. It:

- loads the generated arithmetic, meditation, and combined feature tables;
- validates completeness and participant coverage;
- compares feature distributions and effect sizes;
- analyzes electrode and brain-region patterns;
- checks temporal stability;
- separates train and test data by participant to reduce leakage;
- standardizes the feature matrix;
- trains and evaluates a 100-tree Random Forest;
- calculates Gini and permutation feature importance;
- investigates why the classes separate so strongly.

### 4. Final-report visualization

`a1936769_assignment1D.ipynb` creates explanatory brain-state visualizations used in the final report.

## Feature engineering

The 26 final measures are grouped into:

- frequency-band powers: delta, theta, alpha, beta, and gamma;
- relative band powers;
- cognitive ratios: Alpha/Theta, Beta/Alpha, and Theta/Beta;
- spectral features: dominant frequency, centroid, bandwidth, and spectral-edge measures;
- amplitude and energy features;
- signal-complexity features such as zero crossings and coefficient of variation.

Welch power spectral density estimation converts time-domain EEG into band-level power. Relative powers normalize for baseline amplitude differences, while ratios provide interpretable indicators of relaxed awareness, memory processing, and active concentration.

## Validation strategy

The final workflow splits by participant rather than randomly splitting individual EEG rows. This is important because adjacent samples from the same person are highly related; placing one person's samples in both train and test data would inflate performance.

The model was selected for interpretability and robustness rather than architectural complexity. Feature importance and raw distribution analysis were then used to connect predictions back to neurophysiological mechanisms.

## Repository contents

| File | Purpose |
| --- | --- |
| `kshitij_final_report.pdf` | Final Part D report and authoritative final results |
| `a1936769_assignment1A.ipynb` | Source-data inspection and initial spectral analysis |
| `a1936769_assignment1B.ipynb` | Reusable feature extraction and preliminary ML experiments |
| `a1936769_assignment1C.ipynb` | Expanded EDA, participant split, final Random Forest, and interpretability analysis |
| `a1936769_assignment1D.ipynb` | Final-report visualizations |
| `README.txt` | Historical summary of an earlier, smaller experiment |

## Running the project

Use Python 3 with:

```text
numpy
pandas
scipy
matplotlib
seaborn
scikit-learn
mne
pyedflib
```

Suggested order:

1. Download the OpenNeuro and PhysioNet EEG datasets.
2. Place the BDF/EDF files in the expected raw-data folders.
3. Run Part A to inspect the source files.
4. Run Part B to review and adapt feature extraction.
5. Generate or supply the three CSV feature datasets expected by Part C.
6. Run Part C for the final EDA, split, model, and interpretation.
7. Run Part D to reproduce the final explanatory graphics.

Several outputs are written by Part C, including prepared train/test CSVs, scaled feature matrices, labels, confusion-matrix graphics, and feature-importance plots.

## Interpretation

The final analysis suggests that the two datasets occupy strongly separated feature regimes, with delta power acting as a near-binary separator in the processed samples. The feature differences were reported across multiple brain regions and remained stable through time.

The result is useful as an interpretable proof of concept, but it should not be read as evidence that arbitrary meditation and arithmetic sessions can always be classified perfectly in real-world conditions.

## Limitations

- The cognitive states come from different public datasets, recording systems, participant groups, sampling rates, and session designs.
- Meditation is summarized minute by minute, while arithmetic is summarized second by second.
- Complete delta separation may reflect acquisition or preprocessing differences in addition to genuine cognitive-state effects.
- The final participant counts refer to identifiers in the processed dataset and should be checked against the original source-subject definitions when reproducing the work.
- Laboratory recordings do not capture movement, noise, consumer-grade sensors, or everyday environments.
- Findings may not generalize to other meditation styles, cognitive tasks, age groups, or clinical populations.
- Independent validation on a single protocol that records both tasks from the same participants is the most important next step.

## Future work

The report proposes:

- same-participant and same-hardware validation;
- reduced 3-5 electrode configurations;
- personalized baselines;
- transition-state detection;
- additional cognitive and clinical tasks;
- multimodal physiological sensing;
- real-time meditation feedback and educational attention monitoring.

## Author

Kshitij Desai, COMP SCI 7209 Big Data Analysis, University of Adelaide, August 2025.
