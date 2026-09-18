# Alumni Donor Propensity Forecaster

## Project Overview
The Alumni Donor Propensity Forecaster is a machine-learning-based binary classification project designed to estimate the likelihood that an alumnus will make a donation within the next 12 months.

The project is organized into four Google Colab notebooks. Each notebook has a separate responsibility: dataset generation, model training, model testing, and API implementation.

## Project Workflow

Notebook 1 → `alumni_dataset.csv` → Notebook 2 → `alumni_donor_model_pipeline.pkl` → Notebook 3 → Prediction & Propensity Score → Notebook 4 → API Endpoint

---

# Notebook 1 – Dataset Generator

**Suggested Name:** `Alumni_Donor_Propensity_Forecaster_dataset_generator`

### Purpose
This notebook is used to create the synthetic alumni dataset required for the project.

### Main Work
- Generate 5,000 synthetic alumni records.
- Create demographic, engagement, communication, volunteering, career, and donation-related fields.
- Generate the target variable `donated_next_12_months`.
- Perform donation-related consistency checks.
- Validate the generated dataset.
- Save the dataset as `alumni_dataset.csv`.

### Output
`alumni_dataset.csv`

This file is used as the input for Notebook 2.

---

# Notebook 2 – Model Training

**Suggested Name:** `Alumni_Donor_Propensity_Forecaster_train`

### Purpose
This notebook prepares the dataset, performs feature engineering and preprocessing, trains multiple classification models, evaluates them, and saves the trained model pipeline.

### Main Work
- Load `alumni_dataset.csv`.
- Perform feature engineering.
- Separate input features and target variable.
- Remove `alumni_id` from model features because it is an identifier.
- Split the data into training and testing sets.
- Apply numerical preprocessing using `StandardScaler`.
- Apply categorical preprocessing using `OneHotEncoder`.
- Combine preprocessing using `ColumnTransformer`.
- Train Logistic Regression.
- Train Random Forest.
- Train XGBoost.
- Train CatBoost.
- Evaluate the models using classification metrics.
- Calculate donation probabilities.
- Convert probability into a donor propensity score.
- Save the trained Logistic Regression pipeline using Joblib.

### Output
`alumni_donor_model_pipeline.pkl`

The saved pipeline contains the preprocessing steps and trained model.

---

# Notebook 3 – Model Testing

**Suggested Name:** `Alumni_Donor_Propensity_Forecaster_test`

### Purpose
This notebook verifies that the saved pretrained model works correctly with new alumni input.

### Main Work
- Load `alumni_donor_model_pipeline.pkl`.
- Verify that the model loads successfully.
- Prepare sample alumni input.
- Apply the required feature engineering.
- Generate prediction using `predict()`.
- Generate donation probability using `predict_proba()`.
- Convert probability into a propensity score.
- Categorize the score as High, Medium, or Low.
- Test multiple alumni records.
- Perform practical inference tests.
- Verify the final saved model.

### Propensity Categories

| Propensity Score | Category |
|---|---|
| 80–100 | High |
| 50–79 | Medium |
| 0–49 | Low |

The propensity score represents the model-estimated likelihood and is not a guarantee of donation.

### Input
`alumni_donor_model_pipeline.pkl`

### Output
Prediction, donation probability, propensity score, and propensity category.

---

# Notebook 4 – API

**Suggested Name:** `Alumni_Donor_Propensity_Forecaster_API`

### Purpose
This notebook exposes the trained model through a Flask API so that an external application can send alumni information and receive a prediction.

### Main Work
- Load `alumni_donor_model_pipeline.pkl`.
- Create the Flask application.
- Create the `/predict` POST endpoint.
- Receive alumni information in JSON format.
- Validate the input.
- Prepare the input for the model.
- Generate the prediction.
- Calculate donation probability.
- Convert probability into a propensity score.
- Return the result as JSON.

### Endpoint
`POST /predict`

### Expected Result
The API returns:
- Donation prediction
- Donation probability
- Propensity score
- Propensity category

Notebook 4 uses the pretrained model created in Notebook 2 and does not retrain the model.

---

# Dataset

The project uses a synthetic dataset containing 5,000 alumni records.

Important fields include:

- `alumni_id` – Unique identifier for an alumnus.
- `graduation_year` – Year of graduation.
- `age` – Age of the alumnus.
- `events_attended` – Number of alumni events attended.
- `emails_received` – Number of emails received.
- `emails_opened` – Number of emails opened.
- `newsletter_clicks` – Number of newsletter links clicked.
- `volunteer_events` – Number of volunteering events attended.
- `previous_donations` – Number of previous donations.
- `total_donation_amount` – Total previous donation amount.
- `donation_frequency` – Frequency of previous donations.
- `days_since_last_donation` – Days since the last donation.
- `career_level` – Career-level information.
- `industry` – Industry associated with the alumnus.
- `days_since_last_interaction` – Days since the last interaction.
- `donated_next_12_months` – Target variable indicating whether the alumnus donated within the next 12 months.

---

# Machine Learning Models

The project evaluates four classification algorithms:

### Logistic Regression
Used as an interpretable baseline classification model and for probability estimation.

### Random Forest
Used to capture nonlinear relationships through multiple decision trees.

### XGBoost
Used as a gradient-boosting classification model for learning complex feature relationships.

### CatBoost
Used as another gradient-boosting approach that can work effectively with categorical information.

The models are compared using classification evaluation metrics.

---

# Feature Engineering

Additional features are created to represent alumni engagement and donation behavior more meaningfully.

Examples include:
- Years since graduation
- Email open rate
- Average donation amount
- Donation recency score
- Event engagement
- Newsletter engagement
- Volunteer engagement
- Interaction recency
- Overall engagement score

---

# Model Evaluation

The models are evaluated using:
- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC

Because the dataset is synthetic, the evaluation results demonstrate the behavior of the prototype and should not be treated as real-world donor prediction performance.

---

# Model Serialization

The trained model pipeline is saved as:

`alumni_donor_model_pipeline.pkl`

This allows the preprocessing and trained model to be reused without retraining.

---

# Final Project Files

```text
Alumni_Donor_Propensity_Forecaster/
│
├── Alumni_Donor_Propensity_Forecaster_dataset_generator.ipynb
├── Alumni_Donor_Propensity_Forecaster_train.ipynb
├── Alumni_Donor_Propensity_Forecaster_test.ipynb
├── Alumni_Donor_Propensity_Forecaster_API.ipynb
│
├── alumni_dataset.csv
└── alumni_donor_model_pipeline.pkl
```

---

# How to Run

1. Run **Notebook 1** to generate `alumni_dataset.csv`.
2. Run **Notebook 2** using the generated dataset and create `alumni_donor_model_pipeline.pkl`.
3. Run **Notebook 3** to test the saved pretrained model.
4. Run **Notebook 4** to use the trained model through the API endpoint.

---

# Conclusion

The Alumni Donor Propensity Forecaster demonstrates a complete machine-learning workflow from synthetic data generation to preprocessing, model training, evaluation, pretrained model testing, and API integration. The system uses alumni engagement and historical donation-related information to estimate donation propensity for the next 12 months.
