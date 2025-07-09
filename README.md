# Insider Threat Detection with Azure Synapse and Azure OpenAI

Detect and investigate insider threats using behavioral analytics and generative AI. This project uses Azure Synapse, SynapseML, and Azure OpenAI to build a scalable solution for identifying and analyzing anomalous user behavior.

<!-- ![Azure Architecture Diagram](./images/architecture.png) Optional: Add architecture image -->
---
## 📌 Project Overview
This repository showcases a modern approach to insider threat detection by combining:
- **Machine learning** with SynapseML’s Isolation Forest
- **Natural language insights** with Azure OpenAI to summarize and assess user risk
---
## 🚀 Technologies Used
- **Azure Synapse Analytics (Spark & SQL)**
- **Azure OpenAI (GPT model)**
- **Azure Blob Storage**
- **SynapseML**
- **CMU CERT v4.2 Dataset**
---
## 📁 Dataset
The [CMU CERT v4.2 dataset](https://resources.sei.cmu.edu/library/asset-view.cfm?assetID=508099) simulates realistic insider threat scenarios using synthetic data including:
- `logon.csv` - user logon and logoff events
- `device.csv` - user device usage (example: USB insertion/removal)
- `email.csv` - user email metadata (to, from, content, etc.)
- `http.csv` - user web browsing activity (url
- `file.csv` - user file copy to a removable media device
- `psychometric.csv`\* - user [big 5](http://en.wikipedia.org/wiki/Big_Five_personality_traits) personality data
- `LDAP/` - snapshots of the organizational structure and reporting relationships over time (employee unit, supervisor, role. etc)

**Each file is ingested into Azure Blob Storage and queried through Synapse.**

\* psychometric data was not used as most real-world organizations do not collect or maintain psychometric data on employees. Additionally using personality trait data to infer insider threat risk can raise serious ethical concerns, especially when such inferences are made without behavioral context or consent.

---
## 🧱 Project Structure
📦 insider-threat-detection-synapse-openai

├── data_cleaning/ \
│ ├── clean_device_events.ipynb \
│ ├── clean_email_events.ipynb \
│ ├── clean_file_events.ipynb \
│ └── clean_http_events.ipynb \
│ └── clean_logon_events.ipynb \
│ └── clean_ldap_details.ipynb \
├── model_training_and_inference/ \
│ ├── engineer_model_features.ipynb \
│ └── train_isolation_forest.ipynb \
│ └── aoai_investigate_anomalies.ipynb \
└── README.md

---
## 🔄 Workflow
### 1. **Data Ingestion & Cleaning**
- Load `csv` files into Azure Data Lake Storage Gen2
- Connect the data lake container to Synapse as a linked service
- Explore data with Synapse SQL
- Clean and standardize records using Spark in Synapse Notebooks
- Write the cleaned datasets back to Synapse using Spark
```df_clean.write.mode("overwrite").saveAsTable("clean_<event_type>_events")```

### 2. **Feature Engineering**
- Generate model features from the log datasets

### 3. **Anomaly Detection**
- Train an **Isolation Forest** model using `SynapseML`
- Flag the top 7% of users with most anomalous behavior

### 4. **User Investigation with OpenAI**
- Pull the 200 most recent logs per dataset for a flagged user
- Generate a prompt embedding user activity + features
- Use **Azure OpenAI** to summarize behavior:
 - Timeline of events
 - Anomalies detected
 - Risk assessment
 - Recommendations

---

### ✅ Requirements
* Azure Subscription with:
  * Synapse workspace (with Spark pool)
  * Azure OpenAI resource (GPT-4 or GPT-3.5)
  * Storage account (Blob)
  
### 🛠️ Setup Instructions
1. Clone repo and upload notebooks to into Synapse
2. Upload dataset files into an Azure Blob Storage
3. Link storage container in Synapse Studio
4. Run notebooks sequentially:
  - Ingest and clean data
  - Engineer features
  - Train Isolation Forest model
  - Investigate flagged users using OpenAI

### 📌 Future Work
* Automate the pipeline with Synapse Pipelines or Azure Data Factory
* Add graph-based user behavior modeling
* Experiment with time-series anomaly models (e.g., VAE, LSTM)

### 🔒 Responsible AI Note
This project uses synthetic data and should not be used for production security monitoring without compliance review. When using LLMs for anomaly analysis, consider explainability, bias, and privacy concerns.

### ⚠️ **Disclaimer**
This repository is part of a personal learning project and is not an official Microsoft product or endorsed solution. It uses the CERT Insider Threat Dataset v4.2 from Carnegie Mellon University SEI strictly for research. No raw dataset or derived data is included here. To access the dataset, please visit the [official SEI site](https://resources.sei.cmu.edu/library/asset-view.cfm?assetID=508099).

### 📣 Contact
Simra Ali
💼 [LinkedIn](https://www.linkedin.com/in/simra-ali/)
