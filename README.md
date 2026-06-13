# KKBox Churn Prediction

This is my first real data science project. I'm working through the [WSDM - KKBox's Churn Prediction Challenge](https://www.kaggle.com/c/kkbox-churn-prediction-challenge) on Kaggle, mostly to learn the full process end to end - cleaning messy data, building features, and training a model.

## Goal

KKBox is a music streaming service. The challenge is to predict whether a subscriber is going to churn (i.e. not renew their subscription) based on things like their listening habits, payment history, and basic account info.

## Current status

The main pipeline is done, start to finish:

- explored the raw data and got a feel for what's messy about it (missing values, weird ages, huge files)
- aggregated the giant `user_logs_v2.csv` down to one row per user, using polars since pandas struggled with the size
- did the same for `transactions_v2.csv`
- joined the labels, demographics, transactions, and listening activity into one feature table - one row per user
- trained a baseline LightGBM model on that table

The baseline already does quite well: log loss of ~0.084 (compared to ~0.30 if you just guessed everyone's churn risk as the average rate), and an AUC of ~0.98.

That's where I'm leaving it for now - no hyperparameter tuning, no API. The goal was to get a working model end to end as a first project, and that's done.

## The data

The competition gives you a handful of CSV files:

- `train.csv` / `train_v2.csv` - the labels (which users churned)
- `members.csv` / `members_v3.csv` - user demographics (age, gender, city, registration info)
- `transactions.csv` / `transactions_v2.csv` - subscription and payment history
- `user_logs.csv` / `user_logs_v2.csv` - daily listening activity per user (the big one, ~28GB)

None of this data is in the repo (it's in `.gitignore`) since it's huge and not mine to redistribute. If you want to follow along, grab it from the Kaggle page above.

Two versions of `train`/`transactions`/`user_logs` show up because the competition had two stages. I stuck to the `_v2` files throughout, since they all line up to the same time period.

## Notebooks

- `01_explore.ipynb` - first look at all the files: sizes, columns, missing values, how the tables relate
- `02_user_logs_aggregation.ipynb` - turns `user_logs_v2.csv` into one row per user (polars)
- `03_transactions_aggregation.ipynb` - turns `transactions_v2.csv` into one row per user
- `04_feature_table.ipynb` - joins everything into a single feature table
- `05_baseline_model.ipynb` - trains a LightGBM model and checks how good it is

## Project layout

```
kkbox-churn/
├── data/
│   ├── raw/          # downloaded CSVs (not committed)
│   └── processed/    # cleaned/aggregated data
├── notebooks/        # the notebooks above
├── src/              # reusable code (not started yet)
├── models/           # trained models (not committed)
├── api/              # not started - parked for later
└── reports/          # figures, writeups
```

## Running it locally

```bash
git clone https://github.com/souvik-forge/churn-prediction.git
cd churn-prediction
pip install -r requirements.txt
```

You'll also need to download the competition data into `data/raw/` yourself.

## Roadmap

- [x] Set up the project and explore the data
- [x] Work out a plan for the huge `user_logs` file (polars, and the `_v2` file was small enough anyway)
- [x] Build out a feature table joining all the sources
- [x] Train a baseline model (LightGBM)
- [ ] Wrap it in a small API (deferred for now)

## Why this project

I'm new to both data science and Kaggle, so I picked this competition because it's a good mix of messy real data, a class imbalance problem, and a dataset big enough that I actually have to think about memory and performance.
