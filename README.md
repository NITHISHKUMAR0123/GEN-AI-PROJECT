# GEN-AI-PROJECT
[10:10 pm, 19/2/2026] ~ Nithish Kumar: import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
[10:10 pm, 19/2/2026] ~ Nithish Kumar: from google.colab import files
uploaded = files.upload()
[10:10 pm, 19/2/2026] ~ Nithish Kumar: df = pd.read_excel("diabetes.csv")
df.head()
[10:11 pm, 19/2/2026] ~ Nithish Kumar: print("Shape:", df.shape)
print("\nMissing Values:\n", df.isnull().sum())
print("\nStatistical Summary:\n", df.describe())
[10:11 pm, 19/2/2026] ~ Nithish Kumar: columns = ['Glucose','BloodPressure','SkinThickness','Insulin','BMI']

for col in columns:
    df[col] = df[col].replace(0, np.nan)

df.fillna(df.median(numeric_only=True), inplace=True)

print("Missing values after cleaning:")
print(df.isnull().sum())
[10:11 pm, 19/2/2026] ~ Nithish Kumar: def risk_score(row):
    score = 0
    
    if row['Glucose'] > 140:
        score += 3
    if row['BMI'] > 30:
        score += 2
    if row['Age'] > 45:
        score += 2
        
    return score

df['RiskScore'] = df.apply(risk_score, axis=1)
[10:11 pm, 19/2/2026] ~ Nithish Kumar: def risk_level(score):
    if score >= 5:
        return "High Risk"
    elif score >= 3:
        return "Medium Risk"
    else:
        return "Low Risk"

df['RiskLevel'] = df['RiskScore'].apply(risk_level)

print(df['RiskLevel'].value_counts())
[10:12 pm, 19/2/2026] ~ Nithish Kumar: def recommendation(level):
    if level == "High Risk":
        return "Consult doctor immediately and control sugar."
    elif level == "Medium Risk":
        return "Monitor glucose and exercise daily."
    else:
        return "Maintain healthy lifestyle."

df['Recommendation'] = df['RiskLevel'].apply(recommendation)

df[['Glucose','BMI','Age','RiskLevel','Recommendation']].head()
[10:12 pm, 19/2/2026] ~ Nithish Kumar: df['RiskLevel'].value_counts().plot(kind='bar')
plt.title("Diabetes Risk Distribution")
plt.show()
