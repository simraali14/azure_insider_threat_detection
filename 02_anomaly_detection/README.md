# Anomaly Detection

### Why use an Isolation Forest model?

Isolation Forest is a fast, scalable algorithm that excels at detecting rare behavioral outliers in high-dimensional data, making it ideal for identifying insider threats without requiring labeled examples.

While Isolation Forest is a strong choice for anomaly detection, other models worth exploring include Local Outlier Factor (LOF) and One-Class SVMs. Each of these models offers unique advantages and can provide valuable insights into anomalous user behavior.

---

## Step 1) Feature Engineering ([engineer_model_features.ipynb](engineer_model_features.ipynb))
This notebook extracts user behavior features from five types of activity logs (device, email, file, HTTP, and logon) to support insider threat detection using an anomaly detection model (see: investigate_anomalies.ipynb).

#### 🧠 Feature Engineering Strategy

**Temporal windowing approach to capture behavioral shifts:** 
* 60-day baseline window: Captures typical user behavior in the 60 days prior to the recent window. This helps establish a personalized behavioral norm. A 60-day baseline provides a robust view of a user’s typical behavior, smoothing out short-term fluctuations and capturing consistent patterns.
* 14-day recent window: Captures user behavior in the 14 days leading up to their last recorded event. This reflects short-term activity and is crucial for detecting pre-departure anomalies. A 14-day recent window is short enough to detect sudden behavioral shifts, such as spikes in after-hours activity, external communications, or risky web browsing, that often precede insider threat incidents.

#### 🛠️ Features Include:
* **Volume metrics:** email_sent_count, logon_count, http_request_count, etc
* **Behavioral flags:** after_hours_logon, after_hours_file_access, to_external_email_count, etc
* **Uniqueness metrics:** unique_files_count, unique_url_count, etc
* **Spike ratios**: compare recent vs. baseline activity to quantify unusual surges (ex. logon_spike_ratio, file_access_spike_ratio)

This combination of temporal and behavioral features helps the model differentiate between normal variation and potential insider threats. By computing spike ratios (e.g., recent_logon_count / baseline_logon_count), the model can quantify deviations and prioritize users with the most significant behavioral changes.

---

## Step 2) Model Training ([train_isolation_forest.ipynb](train_isolation_forest.ipynb))

This notebook is designed to train an Isolation Forest model for anomaly detection. The goal is to identify anomalous user behavior that may indicate insider threats. The notebook loads in the engineered feature dataset from the previous step and trains an isolation model, specifying that 7% of the dataset should be flagged as anomalous. This percentage can be adjusted based on your usecase. The result of the models inference on the data is then stored in a dataset that includes each user and their anomalous score (ranges from 0.0 to 1.0, with 1.0 being the most anomalous) and anomalous prediction (0 - non anomalous, 1 - anomalous)
