# Learner Behavior Segmentation for an Online Learning Platform
> Business Analytics Group Project — EdTech · Learner Engagement & Success

## Business Question

> *"What distinct behavioral profiles exist among our learners, and how should content, pacing, or support be tailored to each group?"*

Online learning platforms generate rich behavioral data — video views, quiz attempts, forum posts, login patterns — yet most platforms still treat every learner the same way. This project builds a clustering model that groups learners by engagement and performance behavior, then translates each cluster into an actionable, named persona to support personalized interventions.

---

## Data Sources

| Source | Description |
|---|---|
| **OULAD** (Kaggle) | Open University Learning Analytics Dataset — real learner activity logs, assessment records, and VLE clickstream data |
| **Synthetic behavioral data** | Generated with Python using custom business logic — login frequency, time-of-day pattern, video completion rate, days to first activity, forum participation, and submission timeliness |

Every synthetically generated field is documented in the **Data Dictionary** below.

---

## Learner Personas

The model identifies **3 behavioral clusters**:

| Cluster | Persona | Risk Level | Description |
|---|---|---|---|
| 1 | 🟢 Consistent Achiever | Low | High engagement across all channels, strong performance and healthy pacing |
| 0 | 🟡 Steady Moderate | Medium | Average learner profile with stable but not standout activity |
| 2 | 🔴 At-Risk Disengaged | High | Very low engagement or never-started behavior; highest priority for proactive support |

---

## Project Structure

The project follows an 8-module end-to-end analytics lifecycle:

```
Module 1  Business Understanding & Data Generation
Module 2  Exploratory Data Analysis (EDA)
Module 3  Data Cleaning
Module 4  Feature Engineering         ← 39 selected features from 47 candidates
Module 5  Model Development           ← K-Means, K=3 selected by silhouette score
Module 6  Model Deployment            ← FastAPI on Hugging Face + Streamlit dashboard
Module 7  Model Monitoring            ← Data drift tracking with Evidently
Module 8  Final Report & Presentation
```

---

## Technical Stack

| Layer | Technology |
|---|---|
| Data & Modelling | Python, pandas, scikit-learn, numpy |
| Feature selection | VarianceThreshold + correlation filter → 39 features retained |
| Clustering | K-Means (K=3, chosen by silhouette score) |
| API | FastAPI + Uvicorn, deployed on Hugging Face Spaces |
| Dashboard | Streamlit |
| Deployment | Hugging Face Spaces (Docker), Streamlit Community Cloud |

---

## API

**Base URL:** `https://bachhaovan1906-it5135e.hf.space`

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Health check — confirms the model is loaded |
| GET | `/clusters` | Returns all 3 cluster profiles (for the dashboard) |
| POST | `/predict` | Takes one learner's raw activity → returns their persona |

Interactive documentation: [`/docs`](https://bachhaovan1906-it5135e.hf.space/docs)

### Sample predict request

```json
POST /predict
Content-Type: application/json

{
  "login_weekly": 8,
  "dominant_login_time": "Morning",
  "video_completion_rate": 0.9,
  "days_to_first_activity": 3,
  "submission_timeliness_days": -1.5,
  "forum_posts_count": 5,
  "vle_total_clicks": 450,
  "vle_active_days": 40,
  "assess_n_submitted": 5,
  "assess_mean_score": 82
}
```

### Sample response

```json
{
  "cluster_id": 1,
  "persona": "Consistent Achiever",
  "risk_level": "Low",
  "description": "High engagement and strong performance with relatively healthy pacing.",
  "recommendation": "Offer enrichment content, optional advanced tasks, and peer-mentor opportunities."
}
```

---

## Deliverables

| Deliverable | Link |
|---|---|
| 📖 Data Dictionary | https://docs.google.com/spreadsheets/d/16L-opypZOdobV_n1dHaKLJGNYNeSBqofaPQxRxz2t6E/edit?gid=0#gid=0 |
| 🚀 Demo System | https://bachhaovan1906-it5135e.hf.space/docs |
| 📊 Streamlit Dashboard | *(link to be added after deployment)* |

---

## Repository Structure

```
├── notebooks/
│   └── ba-project-featuring-engineer.ipynb   # Full pipeline: EDA → features → clustering
├── artifacts/
│   ├── kmeans_model.joblib                   # Trained K-Means model
│   ├── scaler.joblib                         # Fitted StandardScaler
│   ├── selected_features.json                # 39 selected feature names
│   ├── persona_map.json                      # Cluster ID → persona name
│   ├── persona_details.json                  # Descriptions + recommendations
│   ├── cluster_profiles.json                 # Feature means per cluster
│   └── training_stats.json                   # Population stats for inference
├── deployment/
│   ├── app.py                                # FastAPI service
│   ├── Dockerfile                            # HF Spaces Docker config
│   └── requirements.txt
└── README.md
```

---

## Team Roles

| Role | Responsibilities | 
|---|---|
| Data Engineer | Synthetic data generation, data dictionary, data cleaning |
| Data Analyst | EDA and visualization of learner behavior |
| ML Engineer — Modeling | Feature engineering and clustering model development |
| ML Engineer — Evaluation | Cluster quality validation and persona definition |
| MLOps Lead | API deployment and monitoring |

- Data dictionary:
https://docs.google.com/spreadsheets/d/16L-opypZOdobV_n1dHaKLJGNYNeSBqofaPQxRxz2t6E/edit?gid=0#gid=0

- Demo system
https://huggingface.co/spaces/bachhaovan1906/IT5135E
