# E-Commerce Customer Analytics Platform

A customer intelligence platform for analyzing e-commerce performance, segmenting customers, predicting customer lifetime value (CLV), identifying churn risk, and supporting data-driven business decisions.


---

## Overview

The E-Commerce Customer Analytics Platform combines data analytics, machine learning, and a web-based dashboard to help businesses understand customer purchasing behavior.

The application uses **Streamlit** for interactive visualizations and **FastAPI** for serving customer analytics and machine-learning predictions. It provides tools for customer segmentation, CLV forecasting, churn-risk estimation, explainable predictions, recommendations, and downloadable reports.

The goal is to turn historical transaction data into actionable customer insights that can support customer retention, marketing, and revenue optimization.
📊 Dashboard Preview

![E-Commerce Customer Analytics Dashboard](assets/Ecommerce%20Customer%20Analytics.png)

The executive dashboard provides an overview of key business metrics, including revenue, customers, orders, average order value, country-wise revenue, revenue trends, high-risk customers, and revenue at risk.
## Key Features

### 1. Executive Dashboard
- Total revenue, customers, orders, and transactions
- Average order value
- Revenue trends over time
- Revenue breakdown by country
- High-risk customer count
- Estimated revenue at risk
- Visual summaries of customer and business performance

### 2. Customer 360
- Individual customer profiles
- Customer purchasing behavior and stored features
- Customer segment and prediction results
- Customer-level analytics
- Downloadable customer reports

### 3. Customer Segmentation
- RFM-based customer analysis
- Customer grouping based on purchasing behavior
- Segment distribution visualizations
- Identification of high-value and less-active customers
- Support for differentiated marketing strategies

RFM represents **Recency, Frequency, and Monetary Value**:
- **Recency:** How recently a customer purchased
- **Frequency:** How often a customer purchases
- **Monetary:** How much a customer spends

### 4. Customer Lifetime Value Prediction
- Forecasts customer value over a 90-day horizon
- Supports customer prioritization
- Helps identify potentially valuable customers
- Provides information for targeted marketing decisions

### 5. Churn Prediction
- Estimates the probability that a customer will stop purchasing
- Identifies customers who may require retention efforts
- Supports proactive customer engagement
- Helps prioritize retention campaigns

### 6. Explainable AI
- SHAP-based explanations for supported predictions
- Identifies features contributing to model outputs
- Improves the interpretability of customer scoring
- Helps users understand why a model produced a prediction

### 7. Customer Recommendations
- Provides recommended next actions for customer engagement
- Supports customer retention and prioritization
- Uses prediction outputs and recommendation logic to inform business actions

### 8. What-if Simulator
- Allows selected customer features to be overridden
- Rescores customers using the prediction API
- Helps explore how changes to customer inputs affect predictions
- Supports interactive analysis of customer behavior

Simulated predictions are exploratory estimates and do not guarantee real-world outcomes.

### 9. Prediction History
- Access to recorded prediction results
- Supports auditing and review of previous scoring requests
- Helps track prediction activity

### 10. Reports and Exports
- Individual customer PDF reports
- Excel workbooks for customer analytics
- Prediction-history Excel exports
- CSV exports where supported by the dashboard

---

## Technology Stack

| Category | Technologies |
|---|---|
| Programming Language | Python |
| Frontend Dashboard | Streamlit |
| Backend API | FastAPI |
| Data Processing | pandas, NumPy |
| Machine Learning | scikit-learn, XGBoost |
| Explainable AI | SHAP |
| Visualization | Plotly |
| Feature Storage | Parquet |
| Testing | pytest |
| Deployment | Docker, Docker Compose |
| Automation | GitHub Actions configuration, subject to validation |

---

## System Architecture

The application separates offline data preparation from online prediction serving.

```text
             Historical Transaction Data
                         |
                         v
                Data Preprocessing
                         |
                         v
                 Feature Engineering
                         |
              +----------+----------+
              |                     |
              v                     v
       Customer Segmentation   Model Training
                                    |
                          +---------+---------+
                          |                   |
                          v                   v
                      CLV Model          Churn Model
                          |                   |
                          +---------+---------+
                                    |
                                    v
                              Feature Store
                                    |
                                    v
                              FastAPI Backend
                                    |
                                    v
                            Streamlit Dashboard
                                    |
                                    v
                     Analytics and Predictions
                                    |
                                    v
                    Reports and Recommendations
```

### How it works

1. Historical transaction data is processed and cleaned.
2. Customer-level features are generated from purchasing behavior.
3. Machine-learning models are trained and evaluated.
4. Prepared features are stored for efficient customer lookup.
5. FastAPI serves customer scoring and analytics requests.
6. Streamlit communicates with the backend and presents results.
7. Predictions can include explanations, recommended actions, and report exports.

The feature-store architecture is intended to avoid repeatedly scanning the complete transaction dataset for each customer prediction.

---

## Installation and Setup

### Prerequisites

- Python compatible with the project's dependency requirements
- Git
- VS Code (recommended)
- Docker and Docker Compose (optional)

**Python compatibility:** The original project documentation specifies Python 3.11 and 3.12. Python 3.14 may require updated dependencies and additional compatibility testing.

### 1. Clone the repository

```bash
git clone https://github.com/supabhinav/Ecommerce-Customer-Analytics.git
cd Ecommerce-Customer-Analytics
```

### 2. Create a virtual environment

For Windows PowerShell, using Python 3.11:

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Verify the Python version:

```powershell
python --version
```

### 3. Install dependencies

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements-dev.txt
```

If dependency installation fails, resolve the specific compatibility issue before proceeding.

### 4. Start the FastAPI backend

Run the following command from the project root:

```powershell
uvicorn src.api.main:app --app-dir project --port 8000
```

The API provides interactive documentation at:

- **Swagger UI:** http://localhost:8000/docs
- **Health endpoint:** http://localhost:8000/v1/health

Keep this terminal running.

### 5. Start the Streamlit dashboard

Open a second terminal in the project root and activate the virtual environment:

```powershell
.\.venv\Scripts\Activate.ps1
```

Launch the dashboard:

```powershell
streamlit run project/app/streamlit_app.py
```

Open the dashboard at:

**http://localhost:8501**

Keep both the API and dashboard running while using the application.

### 6. Run using Docker

If Docker and Docker Compose are installed and the configuration has been validated:

```bash
docker compose up --build
```

---

## API Documentation

The FastAPI backend exposes endpoints for customer scoring, customer information, model metadata, simulation, prediction history, and reports.

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/v1/predict/{customer_id}` | Customer scoring and predictions |
| GET | `/v1/health` | Service health check |
| GET | `/v1/model-info` | Model metadata |
| GET | `/v1/metrics` | Operational metrics |
| GET | `/v1/customers` | List customer identifiers |
| POST | `/v1/simulate/{customer_id}` | What-if simulation |
| GET | `/v1/history` | Prediction history |
| GET | `/v1/customers/{customer_id}/profile` | Retrieve a customer profile |
| GET | `/v1/reports/customer/{customer_id}/pdf` | Generate a customer PDF report |
| GET | `/v1/reports/customers/excel` | Generate a customer analytics workbook |
| GET | `/v1/reports/history/excel` | Export prediction history |

### Example prediction request

```bash
curl -X POST "http://localhost:8000/v1/predict/12748" \
  -H "Content-Type: application/json" \
  -d '{"include_explanations": true}'
```

Replace `12748` with a customer ID returned by the `/v1/customers` endpoint if that example ID is not present in your local feature store.

The API documentation at `/docs` can also be used to execute requests interactively.

---

## Model Performance

| Task | Evaluation Metric | Score |
|---|---|---:|
| Churn Prediction | ROC-AUC | 0.813 |
| Customer Lifetime Value (CLV) Prediction | Spearman Correlation | 0.589 |
| CLV Prediction | Top-Decile Lift | 3.53× |
| Customer Segmentation | Number of Clusters | 3 |



### Key findings

- **Churn prediction:** The reported ROC-AUC of 0.813 indicates good ability to rank customers who churn above customers who remain active.
- **Customer prioritization:** The reported top-decile lift of 3.53× suggests that the highest-ranked 10% of customers have substantially higher future value than the overall average, subject to the original metric definition and evaluation methodology.
- **CLV ranking:** A Spearman correlation of 0.589 indicates a moderate positive relationship between predicted and actual customer-value rankings.
- **Customer segmentation:** Three clusters are reported for grouping customers according to purchasing behavior.

### Evaluation considerations

The original training report also shows a low test-set R² for some CLV models. This indicates that absolute monetary predictions need further validation, even if the model can rank customers reasonably well.

Before presenting these figures as final project achievements, reproduce the results using a documented holdout strategy and verify the data splits, preprocessing, and evaluation code.

---

## Testing and Validation

Run the test suite from the project root:

```powershell
python -m pytest -v
```

If coverage tooling is installed, a coverage report can be generated using:

```powershell
python -m pytest --cov=project/src --cov-report=term-missing
```

### Recommended validation checks

- Verify that the API health endpoint responds successfully.
- Test customer listing and customer profile retrieval.
- Validate customer predictions and output formats.
- Check CLV and churn predictions on known examples.
- Verify SHAP explanations.
- Test the what-if simulator.
- Test PDF and Excel report generation.
- Run the full test suite after code changes.

Test counts, coverage percentages, and passing status should be documented only after measuring them in the current repository.

---
## Author

**Abhinav Jha**  
M.Tech Computer Science and Engineering  
NIT Silchar

**GitHub:** [supabhinav](https://github.com/supabhinav)  
**Project Repository:** [Ecommerce-Customer-Analytics](https://github.com/supabhinav/Ecommerce-Customer-Analytics)
