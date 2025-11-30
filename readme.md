# **Gallstone Clinical Dataset**
This project implements and evaluates a range of machine-learning models on the Gallstone dataset, with the goal of identifying the strongest-performing approach for the `binary classification` task.
# ⭐ **Results Summary**

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Model</th>
      <th>Accuracy Mean</th>
      <th>Accuracy Std</th>
      <th>AUC Mean</th>
      <th>AUC Std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td><strong>SVM (Linear)</strong></td>
      <td><strong>0.808871</strong></td>
      <td><strong>0.087594</strong></td>
      <td><strong>0.882907</strong></td>
      <td><strong>0.060758</strong></td>
    </tr>
    <tr>
      <th>2</th>
      <td>Random Forest</td>
      <td>0.786694</td>
      <td>0.054378</td>
      <td>0.864530</td>
      <td>0.050387</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Logistic Regression</td>
      <td>0.783770</td>
      <td>0.089805</td>
      <td>0.860460</td>
      <td>0.064797</td>
    </tr>
    <tr>
      <th>4</th>
      <td>XGBoost</td>
      <td>0.783669</td>
      <td>0.055013</td>
      <td>0.857255</td>
      <td>0.056884</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Gradient Boosting</td>
      <td>0.777319</td>
      <td>0.068054</td>
      <td>0.849321</td>
      <td>0.049544</td>
    </tr>
    <tr>
      <th>6</th>
      <td>MLP Neural Network</td>
      <td>0.765020</td>
      <td>0.044142</td>
      <td>0.842382</td>
      <td>0.061295</td>
    </tr>
  </tbody>
</table>
</div>
# **Dataset Citation**

> Esen I. et al., “Early prediction of gallstone disease with a machine learning-based method from bioimpedance and laboratory data” 2025.
> [https://pubmed.ncbi.nlm.nih.gov/38394521/](https://pubmed.ncbi.nlm.nih.gov/38394521/)
# **Improvements over Reported Approach**

The original study evaluated models using a single 70/30 train–test split and reported the following results for their best-performing model, the Random Forest Classifier:

| Model         | Precision | Recall  | F-Measure | AUC      | Accuracy |
|---------------|-----------|--------:|----------:|---------:|---------:|
| Random Forest | 0.91      | 0.81    | 0.86      | 0.85     | 0.8542   |

Using a comparable setup—while ensuring a stratified 70/30 split—my strongest model, a Linear SVM, achieved similar performance:

| Model         | Precision | Recall  | F-Measure | AUC      | Accuracy |
|---------------|-----------|--------:|----------:|---------:|---------:|
| SVM           | 0.84      | 0.85    | 0.85      | 0.90     | 0.8437   |

In addition to this comparison, the project also goes beyond the paper’s methodology by applying 10-fold stratified cross-validation, which provides a more reliable and generalizable estimate of model performance. This reduces variance, prevents overreliance on a single split, and gives a stronger measure of how the model is expected to perform on unseen data.

# **How to Use This Repository**

### **1. Install dependencies**

```bash
pip install -r requirements.txt
```

### **2. Run the notebooks in order**

```
01_EDA.ipynb  
02_Baseline_Models.ipynb  
03_Cross_Validation.ipynb  
04_TopK_Features.ipynb
```

* **01_EDA** — dataset exploration, distributions, correlations
* **02_Baseline_Models** — initial model comparison using a stratified train/test split
* **03_Cross_Validation** — 10-fold stratified cross-validation
* **04_TopK_Features** — evaluation of SVM and Random Forest using only the statistically strongest features