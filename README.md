# Predicting Swing-and-Miss in Major League Baseball

## Overview

This project investigates which pitch characteristics are most predictive of a swing-and-miss in Major League Baseball. Using pitch-level Statcast data, I developed and evaluated Random Forest classification models to examine how velocity, movement, location, pitch type, handedness, and count information contribute to predicting whiffs.

The analysis uses MLB pitches from April 1–7, 2024 for model development and a completely separate period from April 15–21, 2024 for external validation.

## Data

Pitch-level Statcast data were collected using the `pybaseball` package.

The modeling dataset was restricted to pitches on which the batter swung:

- **Whiff:** swinging strike or blocked swinging strike
- **Contact:** foul ball, ball put into play, or foul tip
- Bunt attempts were excluded

The development dataset contained 12,082 swings with an approximately 23% whiff rate.

## Methodology

Models were developed incrementally to evaluate how different groups of pitch characteristics contributed to predictive performance:

1. Velocity
2. Velocity + movement
3. Velocity + movement + location
4. Pitch characteristics + pitch type, handedness, and count
5. Final model including an engineered batter-relative vertical location feature

Random Forest models were evaluated using 5-fold cross-validation and ROC-AUC.

A batter-relative vertical location feature was also engineered using Statcast's estimated top and bottom of each batter's strike zone. The final model retained both absolute and batter-relative vertical location.

Permutation importance was used to evaluate which features provided the most predictive information.

## Key Findings

- Velocity alone provided limited predictive ability with a cross-validated ROC-AUC of **0.557**.
- Adding pitch movement did not improve performance, resulting in an ROC-AUC of **0.548**.
- Adding pitch location produced the largest improvement, increasing ROC-AUC to **0.712**.
- Additional contextual information and feature engineering improved the final cross-validated ROC-AUC to **0.735**.
- The frozen final model achieved an ROC-AUC of **0.742** on a completely separate validation period.
- Vertical location produced the largest decrease in model performance when permuted, indicating that it provided the most important predictive information among the features tested.
- Sliders below the batter-specific bottom of the strike zone produced a **57.7% whiff rate**, compared with **21.2%** for sliders at or above the bottom of the zone.

## Conclusion

Among the pitch characteristics evaluated in this analysis, location—particularly vertical location—provided the strongest predictive information for distinguishing whiffs from contact. The model's performance on a separate validation period suggests that the relationships identified during model development generalized to unseen MLB games.

These results describe predictive relationships rather than causal effects and are based on a limited early-season sample.

## Tools

- Python
- pandas
- pybaseball
- scikit-learn
- Matplotlib
- MLB Statcast data

## Repository Contents

- `MLB_Whiff_Prediction.ipynb` — Complete analysis, modeling, visualizations, external validation, and interpretation
