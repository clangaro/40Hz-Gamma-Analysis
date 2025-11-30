# 40Hz Gamma Light Stimulation Analysis

This repository contains the complete statistical analysis pipeline for evaluating whether 
40 Hz gamma-frequency light stimulation alters:

- Circadian rhythms (RA, IV, IS, Amplitude, MESOR, Mean)
- Barnes Maze spatial learning and memory (latency learning curve, distance, errors, quadrant bias)

The analysis includes:
- Mixed-effects linear models (LMM)
- Gamma/Negative Binomial GLMM where appropriate
- Circular statistics for circadian phase
- FDR-corrected hypothesis testing
- Diagnostic plots
- Effect size estimation

## Project Structure
40Hz-Gamma-Analysis/
│
├── README.md
├── requirements.txt
├── analysis.py
│
├── src/
│   ├── clarify_questions.md
│   ├── models_circadian.py
│   ├── models_barnes.py
│   ├── utils.py
│
├── data/
│   ├── circadian.csv
│   ├── barnes.csv
│
├── figures/
└── results/



