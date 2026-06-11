# KKBox Churn Prediction

This is my first real data science project. I'm working through the [WSDM - KKBox's Churn Prediction Challenge](https://www.kaggle.com/c/kkbox-churn-prediction-challenge) on Kaggle, mostly to learn the full process end to end - cleaning messy data, building features, training a model, and (eventually) putting it behind a simple API.

## What's the goal?

KKBox is a music streaming service. The challenge is to predict whether a subscriber is going to churn (i.e. not renew their subscription) based on things like their listening habits, payment history, and basic account info.

## Where things stand

Still early. Right now I'm exploring the data - checking column types, missing values, how big the files are, how the different tables relate to each other, that kind of thing. No model yet, that's the next big step.

## The data

The competition gives you a handful of CSV files:

- `train.csv` / `train_v2.csv` - the labels (which users churned)
- `members.csv` / `members_v3.csv` - user demographics (age, gender, city, registration info)
- `transactions.csv` / `transactions_v2.csv` - subscription and payment history
- `user_logs.csv` / `user_logs_v2.csv` - daily listening activity per user (the big one, ~28GB)

None of this data is in the repo (it's in `.gitignore`) since it's huge and not mine to redistribute. If you want to follow along, grab it from the Kaggle page above.

## Project layout

```
kkbox-churn/
├── data/
│   ├── raw/          # downloaded CSVs (not committed)
│   └── processed/    # cleaned/aggregated data
├── notebooks/        # exploration notebooks
├── src/              # reusable code (data loading, features, models)
├── models/           # trained models (not committed)
├── api/              # FastAPI app for serving predictions (not started)
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
- [ ] Work out a plan for the huge `user_logs` file (probably using polars instead of pandas)
- [ ] Build out a feature table joining all the sources
- [ ] Train a baseline model (likely LightGBM)
- [ ] Wrap it in a small API

## Why this project

I'm new to both data science and Kaggle, so I picked this competition because it's a good mix of messy real data, a class imbalance problem, and a dataset big enough that I actually have to think about memory and performance - not just `pd.read_csv()` and call it a day.
