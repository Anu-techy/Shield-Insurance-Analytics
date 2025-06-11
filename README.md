**Aim:** To develop a predictive model for Shield Insurance to estimate health insurance 
premiums based on factors like age, smoking habits, BMI, and medical history.

**Client:** Shield Insurance 

**Steps:**

1. Data Collection and Preprocessing 
2. Model Development 
3. Model Deployment 
4. Streamlit Application Development 
5. Testing and Validation 
6. Documentation and Training 

**About Data:**

Data is given in premiums.xlsx file by Client

shape : (50000, 13)

columns : 'Age', 'Gender', 'Region', 'Marital_status', 'Number Of Dependants', 'BMI_Category', 'Smoking_Status', 'Employment_Status', 'Income_Level',
       'Income_Lakhs', 'Medical History', 'Insurance_Plan', 'Annual_Premium_Amount'

**Data cleaning**

1. 26 records with null values removed
1. Mininum number_of_dependants is -3, which is inconsistent, Talked to data collection team ,
   and converted 72 -ve values into +ve values.
2. The age column had 58 rows with age above 100, so removed those values by considering 100 as threshold
3. The income_lakhs column ranges from a minimum of 1 lakh to a maximum of 930 lakhs.
   While such extreme values are possible, including them can significantly skew the distribution.

   Since df1.income_lakhs.quantile(0.999) = 100, it indicates that 99.9% of the income values are at or below 100 lakhs,
   with only 0.1% exceeding this threshold. By setting 100 lakhs as the upper threshold,
   10 records with higher income values are removed from the dataset.

**Feature Engineering**
         
1. The 'medical_history' column contains following unique categories:
        'Diabetes', 'High blood pressure', 'No Disease','Diabetes & High blood pressure', 'Thyroid', 'Heart disease',
       'High blood pressure & Heart disease', 'Diabetes & Thyroid','Diabetes & Heart disease'
   Each record contains one disease, or two diseases (separated by " & "). 

   Created a new column **'disease1'** that contains the first disease (or the only disease if there's just one).

2. Created a new column **'disease2'** that contains the second disease, or NaN/None if there’s only one or no disease.

3. Given numerical values for each disease category as: 

       risk_scores = {'Diabetes':6, 'Heart disease':8, 'High blood pressure':6, 'Thyroid':5, 'No Disease':0, "none":0}

   Created a new column, **'total_risk_score'**, that sums the mapped scores from disease1 and disease2.

                   Convert categorical data into numerical format

4. Normalized "total_risk_score" using min-max scaler

5. Label encoding for insurance_plan categories and income_level categories since ordinal data

6. One Hot Encoding for nominal category columns: 'gender','region','marital_status','bmi_category','smoking_status','employment_status'

7. Correlation Analysis and Calculated VIF for Multicolinearity and dropped income_level due to high VIF value




