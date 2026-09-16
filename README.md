# Predicting Hotel Booking Cancellations

## Overview

This project uses Python and machine learning to predict whether a hotel booking will be cancelled at the time the reservation is made.

The project was completed as a **MSIS 502 Final Team Project** by Eva Maheshwari, Hilton Nguyen, Kayla Pham, Serena Mei, and William Somat.

The goal was to determine whether booking information available at the time of reservation could be used to identify cancellation risk and provide insights that could support hotel revenue management decisions.

## Project Goal

Hotel cancellations create uncertainty around room availability, revenue, staffing, and overbooking decisions. Our project asks:

> **Can we predict whether a hotel booking will be cancelled when the booking is made?**

We focused on information that a hotel would actually have available at that point in time, rather than using information recorded after the booking outcome.

## Dataset

The project uses the **Hotel Booking Demand dataset**, originally sourced from Kaggle and based on data from two hotels in Portugal.

* **119,390 original bookings**
* **32 variables**
* City Hotel and Resort Hotel
* Arrival dates from July 2015 through August 2017
* Target variable: `is_canceled`

The dataset includes information about booking timing, customer characteristics, booking channels, room pricing, stay details, previous booking history, and special requests.

## Data Cleaning

Before modeling, we:

* Removed the `company` column due to extensive missing data
* Filled missing `agent` values with `0`
* Filled missing `country` values with `Unknown`
* Filled missing `children` values with `0`
* Removed bookings with unrealistic `adr` values
* Removed bookings with zero adults
* Checked for remaining missing values
* Retained approximately 99.7% of the original dataset

A total of 405 rows were removed during cleaning.

## Exploratory Data Analysis

We analyzed several factors associated with cancellation behavior, including:

* Hotel type
* Booking lead time
* Booking channel
* Stay type
* Deposit type
* Special requests
* Repeat guest status
* Seasonality and room pricing

### Key Findings

**Lead time was one of the strongest signals.** Cancelled bookings had a median lead time of 113 days compared with 45 days for bookings that were completed.

**Booking source also showed substantial differences.** Direct bookings had a 15.4% cancellation rate, compared with 36.7% for online travel agents and 61.1% for group bookings.

**Special requests were associated with lower cancellation rates.** Bookings with no special requests had a 47.8% cancellation rate, compared with 22.0% for bookings with one request.

**Repeat guests cancelled less frequently.** Repeat guests had a 14.7% cancellation rate compared with 37.8% for first-time guests.

**Cancellation patterns varied seasonally.** Cancellation rates ranged from 30.5% in January to 41.5% in June, while average daily rates were substantially higher during the summer.

## Predictive Modeling

We developed two classification models:

### Decision Tree

The first model was a Decision Tree with a maximum depth of 3.

The limited depth was intentional. It allowed us to create a model that was easier to interpret and explain to a business stakeholder.

**Test Accuracy: 76.8%**

The most influential splits centered around:

* Deposit type
* Lead time
* Previous cancellations

### Random Forest

The second model was a Random Forest consisting of 100 decision trees.

The Random Forest provided higher predictive accuracy but was less interpretable than the single Decision Tree.

**Test Accuracy: 84.3%**

The most important features included:

1. Lead time
2. Average daily rate (`adr`)
3. Deposit type
4. Number of special requests

## Model Evaluation

We compared both models against a simple baseline that always predicted a booking would not be cancelled.

| Model         | Test Accuracy |
| ------------- | ------------: |
| Baseline      |         63.2% |
| Decision Tree |         76.8% |
| Random Forest |         84.3% |

We also examined the Random Forest's confusion matrix to understand the difference between correctly identifying cancellations, missing cancellations, and incorrectly flagging bookings.

This was important because the two types of prediction errors have different business consequences. Missing a cancellation can leave a room empty, while incorrectly predicting a cancellation could contribute to an overbooking situation.

## Data Leakage

A major part of the modeling process was ensuring that the model only used information available when a booking was made.

We excluded:

* `reservation_status`
* `reservation_status_date`

These variables contain information about the eventual booking outcome and would allow the model to effectively see the answer in advance.

## Business Applications

The analysis can support several hotel functions:

* **Revenue Management:** Improve expectations around future room availability and cancellation risk
* **Marketing:** Understand differences between direct, group, and online travel bookings
* **Operations:** Plan staffing around expected arrivals rather than raw booking counts
* **Customer Retention:** Identify bookings that may benefit from reminders or other retention efforts

## Limitations

The dataset represents only two hotels in Portugal between 2015 and 2017, so the findings may not generalize to other properties, locations, or current booking behavior.

The model also evaluates accuracy rather than the financial cost of each type of prediction error. A future model could incorporate the actual financial impact of empty rooms and overbooking situations.

Additionally, the timing of certain variables, such as parking requests, may need to be verified before using the model in a live booking environment.

## Tools & Technologies

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Jupyter Notebook
* Classification
* Decision Trees
* Random Forest
* Exploratory Data Analysis

## Project Takeaway

This project demonstrates how historical booking data can be transformed into predictive insights that support real-world business decisions. Beyond building a model, the project focused on understanding **why** cancellations happen, evaluating the trade-offs between model accuracy and interpretability, and considering how predictions could be used by hotel decision-makers.
