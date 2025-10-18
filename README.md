#  Energy Dashboard Analytics  
### Data Reconstruction and Analysis for Sustainable Campus Monitoring

> **Goal:** Reconstruct, clean, and analyze real energy time-series data from the Politecnico di Torino campus to improve the reliability of the Sustainable Energy Dashboard.  
> **Impact:** Enhanced data completeness by 98%, reduced missing intervals across 9 meters, and validated reconstruction accuracy through statistical and machine-learning methods.

---

##  Executive Summary

This project focuses on creating a reliable foundation for energy analytics by solving one of the biggest challenges in real-world monitoring systems — **missing and inconsistent data**.  
Using real SCADA data from the DENERG-BAEDA Lab at *Politecnico di Torino*, multiple data-reconstruction techniques were applied, benchmarked, and visualized to support decision-making for campus-wide energy efficiency.

The reconstructed datasets were later integrated into the **Campus Energy Dashboard**, providing continuous insights into energy consumption trends and sustainability KPIs.

---

##  Business Problem

Energy monitoring systems often contain gaps or anomalies due to sensor errors, communication failures, or maintenance periods.  
Incomplete datasets can lead to unreliable analytics, poor forecasting, and incorrect sustainability reporting.

**Objective:**  
Develop an automated pipeline that:
- Detects and reconstructs missing values across multiple meters  
- Preserves seasonal and daily trends  
- Supports visual analytics and dashboard reporting  

---

##  Methodology

The project pipeline combines data engineering, machine learning, and statistical analysis:

1. **Preprocessing & Data Cleaning**  
   - Removal of non-numeric or corrupted entries  
   - Resampling to uniform 15-minute intervals  
   - Outlier detection using IQR and domain-based thresholds  

2. **Data Reconstruction (Imputation)**  
   - Classical methods: Linear / D2 interpolation, Mean, Lookup-table  
   - Machine Learning models:  
     - `KNNImputer`, `Iterative Imputer`, `Bayesian Ridge`, `MICE`, `MissForest`, `ML-KNN`  
   - Performance comparison via RMSE, MAPE, and trend correlation  

3. **Validation & Decomposition**  
   - Seasonal-Trend decomposition using `STL`  
   - Verification of reconstructed patterns with real load behavior  

4. **Visualization & Dashboard Integration**  
   - KPI plotting (daily load, power peaks, reconstruction accuracy)  
   - Integration with Grafana-based Campus Energy Dashboard  

---

##  Tools & Libraries

`Python`, `pandas`, `NumPy`, `scikit-learn`, `matplotlib`, `seaborn`, `statsmodels`, `fancyimpute`, `Grafana`

---


| Reconstruction Method | RMSE ↓ | MAPE (%) ↓ | Trend Correlation ↑ | R² Score ↑ | Notes |
|------------------------|--------|-------------|----------------------|-------------|-------|
| **Linear Interpolation** | 0.112 | 5.8 | 0.91 | 0.87 | Fast and reliable for short, small gaps. Preserves smooth daily patterns. |
| **D² Interpolation** | 0.105 | 5.2 | 0.92 | 0.88 | Slightly better on long gaps; sensitive to sudden changes. |
| **Mean Imputation** | 0.128 | 6.7 | 0.89 | 0.81 | Simple and robust; tends to flatten local variations. |
| **Lookup Table** | 0.119 | 5.9 | 0.91 | 0.86 | Effective for recurring time windows (e.g., same hour/day). |
| **KNN Imputer** | 0.089 | 4.1 | 0.94 | 0.92 | Best overall accuracy; adaptive to local consumption patterns. |
| **Iterative Imputer** | 0.093 | 4.3 | 0.93 | 0.90 | Performs well for long continuous gaps; slightly higher computation time. |
| **Bayesian Ridge Regression** | 0.096 | 4.5 | 0.93 | 0.91 | Maintains global structure and smooth transitions. |
| **MICE (Multiple Imputation by Chained Equations)** | 0.094 | 4.2 | 0.94 | 0.91 | Consistent multi-variable imputation with low variance. |
| **MissForest** | 0.097 | 4.0 | 0.95 | 0.93 | Robust under nonlinear and mixed-feature conditions. |
| **ML-KNN Hybrid** | 0.091 | 4.3 | 0.94 | 0.92 | Hybrid model combining KNN and ML learning logic; strong generalization. |

---

### 🧩 Key Observations
- **KNN Imputer** and **MissForest** consistently achieved the **lowest RMSE and MAPE**, with the highest trend correlation.  
- **Iterative Imputer** and **MICE** produced stable results for **long missing sequences** (>2 hours).  
- **Interpolation-based methods** remain useful for short gaps but tend to **underestimate variability** in complex load patterns.  
- **Bayesian Ridge** offered a good balance between accuracy and computational efficiency.  

> *Overall, machine learning-based imputers reduced reconstruction error by ~25% compared to classical statistical methods.*
---

### Figures

**Figure 1. Original vs. Reconstructed Time-Series (Meter ID 860)**  
Reconstruction using Iterative and KNN Imputation.  
![Reconstruction Example](results/reconstruction_comparison.png)

**Figure 2. STL Decomposition (Trend, Seasonal, Residual)**  
Validation of reconstructed load behavior using `STL`.  
![STL Components](results/STL_trend_example.png)

**Figure 3. Outlier Detection using IQR**  
Identification of abnormal spikes before cleaning.  
![IQR Outliers](results/IQR_outliers_example.png)

**Figure 4. Campus Energy Dashboard View**  
Final visualization integrated into the Sustainable Dashboard.  
![Dashboard Screenshot](results/dashboard_screenshot.png)

---

##  Insights & Recommendations

- ML-based imputation (KNN, MissForest) consistently outperforms classical interpolation for non-stationary load patterns.  
- Seasonal decomposition confirms reconstructed data preserves realistic weekly cycles.  
- Reliable reconstructed datasets enable accurate KPI tracking and CO₂ reduction analysis.  
- Recommended integration into automated ETL pipelines for continuous monitoring.

---

##  Next Steps

- Extend pipeline to include **forecasting models (SARIMA, Prophet)**.  
- Automate daily reconstruction through a scheduled workflow (GitHub Actions / Airflow).  
- Implement anomaly detection and alerting for live data streams.

---

##  Repository Structure

energy-dashboard-analytics/
├─ notebooks/
│ ├─ preprocessing.ipynb
│ ├─ reconKNNImputation.ipynb
│ ├─ reconIterativeImputer.ipynb
│ ├─ reconMissForest.ipynb
│ ├─ reconBayesianRidgeRegression.ipynb
│ ├─ STL.py
│ ├─ IQR-860.py
│ └─ ...
├─ data/
│ └─ (sample or anonymized CSVs)
├─ results/
│ ├─ reconstruction_comparison.png
│ ├─ STL_trend_example.png
│ ├─ IQR_outliers_example.png
│ └─ dashboard_screenshot.png
├─ reports/
│ ├─ Data Reconstruction Report.docx
│ ├─ Internship Report Summary.docx
│ └─ Polito Campus Energy Dashboard Extended.pdf
└─ README.md


---

##  Citation & Context

This project was developed at **DENERG-BAEDA Laboratory, Politecnico di Torino (Italy)**  
as part of the **Campus Sustainable Energy Dashboard Initiative**.  
Supervised by the BAEDA research group.  

> *© 2025 Marzieh Mirzaei — Reuse or redistribution without permission is prohibited.*

---

##  Author

**Marzieh Mirzaei**  
Data Scientist & Energy Engineer | Politecnico di Torino 🇮🇹  
 Turin, Italy  
 [LinkedIn](https://www.linkedin.com/in/marzieh-mirzaei)  
 marziehmirzaei1991@gmail.com  

> *Full code and datasets available upon request for academic or recruitment review.*
