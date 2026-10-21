# Comparing Cross-Validation Techniques for Bank Marketing Prediction

A Portuguese bank phones clients to sell a term deposit, and fewer than 1 in 8 calls ends in a subscription. Before trusting a model that ranks clients, we need an honest estimate of how well it will perform on clients it has never seen. This project compares **ten variants of cross-validation** and checks which estimates come closest to what happens in a later period.

The full analysis is in [`bank_marketing_cross_validation.ipynb`](bank_marketing_cross_validation.ipynb), already executed so the figures show on GitHub.

## Questions

1. Do different cross-validation techniques give different performance estimates for the same model?
2. Which estimate is closest to what the model delivers on future data?
3. How do the techniques differ in bias, variance and computational cost?
4. Do the conclusions hold on a second dataset (telecom customer churn)?

## Main findings

- **Techniques that shuffle rows agree.** For a given model, mean ROC-AUC differs by less than 0.01 between K-Fold, Stratified K-Fold, Repeated and Monte Carlo, which is less than the fold-to-fold variation.
- **A single hold-out is the noisy one.** Its estimate moves 3 to 4 times more across repeats and its typical error is about 1.6 times larger.
- **More computation buys nothing measurable.** Repeating the CV costs 3 to 4 times more, and leave-one-out about 80 times more, without a visible gain in accuracy.
- **The data changes inside the file.** `bank-full.csv` is ordered by date, and from mid-2009 the monthly volume drops sharply and the subscription rate rises from about 7% to about 45%. Random-split CV (about 0.70 AUC) and time-aware CV (about 0.55 AUC) disagree by about 0.15, and neither matches the future exactly. Judged inside each period, the future AUC was 0.57 to 0.65.
- **Leakage matters more than the technique.** Using the call duration, which is only known after the call, lifts AUC from 0.71 to 0.95. It is excluded from the models.
- **Modest business value.** On the future period, calling the top 30% of ranked clients reaches 49% of subscribers overall, but 35% in the later part of the period (random calling would reach 30%).

## Limitations

Models are not tuned (tuning would need nested cross-validation). The time analysis rests on one chronological cut of one dataset, and the calendar was inferred from the order of the months because the file has no year column. Fold standard deviations are not standard errors.

## How to run

1. Install the requirements: `pip install -r requirements.txt`
2. Download the data and place these files in a folder named `data` next to the notebook:
   - `bank-full.csv` and `bank.csv` from the [Bank Marketing dataset](https://archive.ics.uci.edu/dataset/222/bank+marketing) (UCI Machine Learning Repository)
   - `WA_Fn-UseC_-Telco-Customer-Churn.csv` from the [Telco Customer Churn dataset](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) (Kaggle)
3. Open the notebook with Jupyter and run all cells. A full run takes about 9 minutes on 2 CPU cores.

## Data and citation

The data files are not included in this repository. Please cite the authors of the bank dataset:

S. Moro, R. Laureano and P. Cortez. *Using Data Mining for Bank Direct Marketing: An Application of the CRISP-DM Methodology.* Proceedings of the European Simulation and Modelling Conference (ESM'2011), pp. 117-121, Guimaraes, Portugal, October 2011. EUROSIS.

## Author

Rebeca Freitas
