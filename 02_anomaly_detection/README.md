# Anomaly Detection

### Why use an Isolation Forest model?

1. **Anomaly Detection:** Isolation Forests are specifically designed for anomaly detection. They randomly select features and split values to separate observations, making it easier to detect anomalies.

2. **Efficiency:** The algorithm is computationally efficient and can handle large datasets, which is crucial for processing extensive logs and user activity data.

3. **Scalability:** Isolation Forests scale well with the number of data points and dimensions, making them suitable for high-dimensional data typically involved in insider threat detection.

While Isolation Forest is a strong choice for anomaly detection, other models worth exploring include Local Outlier Factor (LOF), One-Class SVM, and Autoencoders. Each of these models offers unique advantages and can provide valuable insights into anomalous user behavior.

---

## Step 1) Feature Engineering ([engineer_model_features.ipynb](engineer_model_features.ipynb))
This notebook extracts user behavior features from five types of activity logs (device, email, file, HTTP, and logon) to support insider threat detection using an anomaly detection model (see: investigate_anomalies.ipynb).

#### 🧠 Feature Engineering Strategy

**Temporal windowing approach to capture behavioral shifts:** 
* 14-day recent window: Captures user behavior in the 14 days leading up to their last recorded event. This reflects short-term activity and is crucial for detecting pre-departure anomalies.
* 60-day baseline window: Captures typical user behavior in the 60 days prior to the recent window. This helps establish a personalized behavioral norm.

This strategy enables us to detect deviations from a user's baseline — a key signal for insider threats, especially for users who may engage in risky actions shortly before exiting the organization.

#### 🛠️ Features Include:
* **Volume metrics:** email_sent_count, logon_count, http_request_count, etc
* **Behavioral flags:** after_hours_logon, after_hours_file_access, to_external_email_count, etc
* **Uniqueness metrics:** unique_files_count, unique_url_count, etc
* **Spike ratios**: compare recent vs. baseline activity to quantify unusual surges (ex. logon_spike_ratio, file_access_spike_ratio)

This combination of temporal and behavioral features helps the model differentiate between normal variation and potential insider threats.

---


---

## Step 2) Model Training ([train_isolation_forest.ipynb](train_isolation_forest.ipynb))

This notebook is designed to train an Isolation Forest model for anomaly detection. The goal is to identify anomalous user behavior that may indicate insider threats. The notebook loads in the engineered feature dataset from the previous step and trains an isolatin model, specifying that 7% of the dataset should be flagged as anomalous. This percentage can be adjusted based on your usecase. The result of the models inference on the data is then stored in a dataset that includes each user and their anomalous score (ranges from 0.0 to 1.0, with 1.0 being the most anomalous) and anomalous prediction (0 - non anomalous, 1 - anomalous)
