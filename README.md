### Comprehensive Project Observations & Insights

This section outlines the end-to-end observations derived from the project, ranging from initial data quality assessment to final model generalization.

#### 1. Data Quality & Statistical Characteristics

The initial inspection revealed that the dataset is sufficiently large for machine learning but required significant cleaning and understanding of underlying statistics.

* **Data Composition:** The dataset contains a mix of numerical and categorical features.
* **Class Imbalance:** A distinct class imbalance was observed in the target variable (0: Repaid vs. 1: Default).
  <img width="560" height="391" alt="image" src="https://github.com/user-attachments/assets/8b7e996b-bc93-43e1-9ccf-a6d0fcc09218" />

* **Observation:** The "Repaid" class dominates with approximately **280,000** records, while the "Default" class accounts for only about **25,000** records. This results in a roughly **91.8% to 8.2%** split, necessitating robust handling techniques. * **Statistical Distribution:**
* **Skewness:** Many numerical features followed a right-skewed distribution.
* **Outliers:** The `AMT_INCOME_TOTAL` variable displayed extreme outliers. While the majority of incomes are compressed near the lower range (), extreme values extended up to ****, confirming the need for log transformations to stabilize variance.

<img width="794" height="476" alt="image" src="https://github.com/user-attachments/assets/eeba2f2b-300b-4260-847d-e84695ce38b4" />



* **Age Distribution:** Applicant age follows a generally normal distribution, peaking between **30 and 40 years old**, with a sharp decline in applicants over 60.

    <img width="863" height="468" alt="image" src="https://github.com/user-attachments/assets/ed0e602d-b724-48c7-97b5-f0690b45065c" />

#### 2. Socioeconomic & Demographic Risk Profiling

Visual analysis of the applicant base revealed clear distinctions in default probability based on education, family status, and asset ownership.

* **Education Levels:** There is a clear inverse relationship between education level and default risk.
* **Highest Risk:** Applicants with "Lower Secondary" education have the highest probability of default, exceeding **11%**.
* **Lowest Risk:** Applicants with an "Academic degree" have the lowest probability of default (under **2%**).
  <img width="1006" height="598" alt="image" src="https://github.com/user-attachments/assets/62435f39-13e1-4735-b98a-bb754bfd99af" />



* **Family Status:**
* **Risk Factors:** "Civil marriage" (**9.9%**) and "Single / not married" (**9.8%**) statuses show the highest default probabilities.
* **Stability:** "Widows" represent the safest category with a default rate of only **5.8%**.
  <img width="1335" height="1152" alt="image" src="https://github.com/user-attachments/assets/7f50580c-49e1-42b3-a05a-aac4d09b23f3" />



* **Asset Ownership:**
* **Real Estate vs. Cars:** Real estate ownership is more common (~213k applicants) than car ownership (~104k applicants).
* **Portfolio:** The most common asset profile is owning **Realty Only** (~140k applicants).
* **Housing Profile:** A radar chart analysis of housing features (Living Area, Elevators, Entrances) indicates that Repayers consistently score higher on housing quality metrics compared to Defaulters, though the shape of the profile remains similar.
  <img width="1335" height="1071" alt="image" src="https://github.com/user-attachments/assets/2b1be1e7-f161-40da-819f-8c7121e55750" />




#### 3. Feature Relationships & Engineering

Understanding how features interact with each other and the target variable was critical for feature selection.

* **Correlation Analysis:**
* **Key Findings:** The heatmap revealed a strong negative correlation (**-0.60**) between `DAYS_BIRTH` and `EXT_SOURCE_1`.
* **Multicollinearity:** Weak positive correlations were observed between `AMT_CREDIT` and `AMT_INCOME_TOTAL` (**0.16**). Direct correlations with the `TARGET` variable remained low across the board (all < 0.2), suggesting that no single feature is a "silver bullet" predictor. * **Transformations:**
* **Normalization:** Due to violated normal distribution assumptions (evidenced by the income boxplots), log transformations and scaling were necessary.
* **Encoding:** Categorical variables were dominated by specific categories (e.g., "Married" status accounts for ~190k records) and required Label or One-Hot Encoding.
<img width="899" height="809" alt="image" src="https://github.com/user-attachments/assets/fe84d359-181f-40f5-adce-27ddc454d1d1" />



#### 4. Machine Learning Modeling Strategy & Training Dynamics

The transition from data analysis to modeling highlighted the importance of monitoring loss curves to prevent overfitting.

* **Training Dynamics:**
* **Loss Curves:** The model shows a sharp decrease in Train Loss (dropping from **0.284 to ~0.245**). However, Validation Loss plateaus and begins to fluctuate around **0.252** after Epoch 6, indicating the onset of overfitting.
* **AUC Performance:** The Model AUC (Area Under Curve) confirms this trend. Train AUC climbs steadily to **~0.76**, while Validation AUC stabilizes around **0.748**, creating a widening gap that suggests the model is memorizing training data rather than generalizing. * **Regularization:** Techniques to penalize complexity were essential to narrow the gap between the training and validation performance shown in the learning curves.



#### 5. Performance Evaluation & Class Imbalance

Given the class imbalance identified in the data analysis phase, standard accuracy was deemed an insufficient metric.

* **Metric Selection:** Evaluation prioritized Precision, Recall, F1-score, and ROC-AUC.
* **Handling Imbalance:** Since the dataset is heavily skewed (approx 11:1 ratio), models initially favored the majority class. Implementing resampling techniques and adjusting class weights were necessary.
* **Trade-offs:** A clear trade-off between precision and recall was observed; recall was prioritized in scenarios where missing a positive instance (False Negative) was costly.
<img width="506" height="468" alt="image" src="https://github.com/user-attachments/assets/4ad2613a-96f8-4fa7-ba16-ecaae7f3e565" />
<img width="499" height="545" alt="image" src="https://github.com/user-attachments/assets/4c4db215-0de8-48cb-b587-0b934d5c78bb" />



#### 💡 Final Conclusion

The project demonstrates that Data Preprocessing (handling missing values, scaling, encoding) had a greater impact on final performance than model selection alone. The statistical analysis highlighted that while **Education** and **Family Status** are strong determinants of risk, the model struggles with overfitting after approximately **6 to 8 epochs**. Future iterations should focus on stronger regularization or early stopping to maintain the Validation AUC around the 0.75 mark while reducing the generalization gap.
