EEG Cognitive State Classification Project
=========================================

Assignment 1B: Big Data Analysis
Student: Kshitij Desai (A1936769)
Course: COMP SCI 7209 - Big Data Analysis

Project Overview:
-----------------
Machine learning classification of EEG patterns to distinguish between meditation 
and mathematical thinking cognitive states using Random Forest algorithms.

Dataset:
--------
- Meditation EEG: 3 files from OpenNeuro (ds001787), 256 Hz, 44.5 min average
- Arithmetic EEG: 3 files from PhysioNet (EEGMAT), 500 Hz, 3.0 min average
- Total: 24 feature vectors (12 meditation + 12 arithmetic)

File Structure:
===============

filea.ipynb - DATA PREPROCESSING
--------------------------------
• EEG data loading (BDF/EDF files)
• Signal quality assessment
• Feature extraction (13 neurophysiological features):
  - Frequency band powers (Delta, Theta, Alpha, Beta, Gamma)
  - Relative power ratios
  - Cognitive state ratios (Alpha/Theta, Beta/Alpha)
  - Spectral characteristics
  - Signal complexity measures
• Data cleaning and standardization
• Sampling rate harmonization (256 Hz vs 500 Hz)

fileb.ipynb - ANALYSIS & RESULTS
---------------------------------
• K-means clustering (83.33% accuracy)
• Principal Component Analysis (80.9% variance explained)
• Random Forest classification (96% ± 8% accuracy)
• Statistical significance testing (t-tests)
• Feature importance analysis
• Comprehensive visualizations (4-panel figure)
• Results interpretation and discussion

Key Results:
============
• 96% classification accuracy achieved
• Most important features: Theta relative power (23.8%), Alpha power (21.5%)
• Meditation: Higher Alpha/Theta ratio (3.43 vs 1.19)
• Arithmetic: Higher theta activity (11.5% vs 1.9%)

Output Files:
=============
• cognitive_analysis_plots.png - Main analysis figure
• latex.txt - Complete academic report
• README.txt - This file

Dependencies:
=============
• Python 3.11
• pandas, numpy, matplotlib, seaborn
• scikit-learn
• pyedflib (for EEG file reading)
• scipy

Usage:
======
1. Run a1936769_assignment1A.ipynb for data preprocessing
2. Run a1936769_assignment1B.ipynb for analysis and results