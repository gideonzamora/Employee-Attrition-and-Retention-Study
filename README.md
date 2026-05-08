# Employee Dataset Mini Case Study

### Business Task
- This analysis aims to identify the key drivers of employee attrition. By examining compensation, experience, and workforce distribution, we uncover patterns that can help improve retention strategies

### Dataset
- This dataset contains employee-level information used to analyze workforce trends and identify key factors influencing employee attrition. It includes demographic details such as age, gender, and education, along with job-related attributes like city, payment tier, years of experience, and employment history indicators such as whether an employee has ever been benched. [View Dataset](https://www.kaggle.com/datasets/tawfikelmetwally/employee-dataset)

### Skills
- Data Cleaning & Standardization
- Aggregate Functions (COUNT, AVG, SUM)
- SQL Clauses and Expressions (Group by, Order by, Where, Case)
- Subqueries
- Exploratory Data Analysis (EDA)

### Tools
- MySQL

## Business Questions and Insights

1. How does employee attrition vary across different payment tiers?
```sql
SELECT 
	payment_tier,
    COUNT(*) AS total_employees,
    SUM(resigned) as resigned,
    ROUND((SUM(resigned) / COUNT(*))*100, 2) AS attrition_rate
FROM
	employee_dataset
GROUP BY
	payment_tier
ORDER BY
	attrition_rate DESC
;
```

**Key Insight:**

<img width="395" height="92" alt="image" src="https://github.com/user-attachments/assets/9f9ea0fc-9bf0-403c-aeca-e2bc101a4955" />
<br>
Employees in Payment Tier 2 have the highest attrition rate at nearly 60%, significantly higher than all other groups.

***

2. Which cities experience the highest employee attrition rates?
```sql
SELECT 
	city,
    COUNT(*) AS total_employees,
    SUM(resigned) as resigned,
    ROUND((SUM(resigned) / COUNT(*))*100, 2) AS attrition_rate
FROM
	employee_dataset
GROUP BY
	city
ORDER BY
	attrition_rate DESC
;
```

**Key Insight:**

<img width="373" height="90" alt="image" src="https://github.com/user-attachments/assets/1a4fa31c-33d4-4323-a745-9f9cf6d435dd" />
<br>
Employees in Pune have an attrition rate of 50.39%, nearly double that of Bangalore.

***

3. Which city has the most experienced workforce?
```sql
SELECT 
	CASE
		WHEN experience <= 1 THEN '0-1'
        WHEN experience BETWEEN 2 AND 4 THEN '2-4'
        ELSE '5+'
	END AS experience_range,
    city,
    COUNT(*) AS total_employees
FROM
	employee_dataset
GROUP BY
	experience_range,
    city
ORDER BY
	experience_range
;
```

**Key Insight:**

<img width="330" height="216" alt="image" src="https://github.com/user-attachments/assets/9a0d542d-e282-4999-b7ab-706637a571d0" />
<br>
Bangalore leads in all experience categories, including the highest number of employees with 5+ years of experience. However, it does not tell us the whole story.
<br>
<br>
3.1. What is the percentage of experienced employees relative to the total headcount of employees in each city?

```sql
SELECT 
    city,
    COUNT(*) AS total_employees,
    SUM(CASE WHEN experience >= 5 THEN 1 ELSE 0 END) AS experienced,
    ROUND(SUM(CASE WHEN experience >= 5 THEN 1 ELSE 0 END)*100.0 / COUNT(*), 2) AS experienced_percentage
FROM 
	employee_dataset
GROUP BY 
	city
ORDER BY 
	experienced_percentage DESC
;
```

<img width="467" height="88" alt="image" src="https://github.com/user-attachments/assets/7689c8b5-435d-4bcc-bb5a-ffc91d21d56a" />
<br>
Although Bangalore has the highest numbers, the proportion of experienced talent is relatively consistent across cities, indicating that workforce seniority distribution is balanced

***

4. Does education level influence employee attrition?
```sql
SELECT 
	education,
    COUNT(*) AS total_employees,
    SUM(resigned) as resigned,
    ROUND((SUM(resigned) / COUNT(*))*100, 2) AS attrition_rate
FROM
	employee_dataset
GROUP BY
	education
ORDER BY
	attrition_rate DESC
;
```

**Key Insight:**

<img width="372" height="87" alt="image" src="https://github.com/user-attachments/assets/462a5788-143e-406a-95d1-90f9ea940cb4" />
<br>
The table shows that the masters degree holders as the group with the highest percentage of attrition.

***

5. How does attrition differ across age groups?
```sql
SELECT
	CASE
		WHEN age < 25 THEN 'Under 25'
        WHEN age BETWEEN 25 AND 29 THEN '25-29'
        WHEN age BETWEEN 30 AND 34 THEN '30-34'
        WHEN age BETWEEN 35 AND 39 THEN '35-39'
        ELSE '40+'
	END AS age_group,
    COUNT(*) AS total,
    ROUND(AVG(resigned)*100, 2) as attrition_rate
FROM
	employee_dataset
GROUP BY
	age_group
;
```
**Key Insight:**

<img width="238" height="132" alt="image" src="https://github.com/user-attachments/assets/fd87369e-4136-455d-a1ba-315a5ee57601" />
<br>
The data identifies that employees under the age of 25 have the highest attrition rate of 39%.

***

6. What is the relationship between years of experience and attrition?
```sql
SELECT
	CASE
		WHEN experience <= 1 THEN '0-1'
        WHEN experience BETWEEN 2 AND 4 THEN '2-4'
        ELSE '5+'
	END AS experience_range,
    COUNT(*) AS total,
    ROUND(AVG(resigned)*100, 2) as attrition_rate
FROM
	employee_dataset
GROUP BY
	experience_range
ORDER BY
	attrition_rate DESC
;
```
**Key Insight:**

<img width="281" height="88" alt="image" src="https://github.com/user-attachments/assets/b6ed9196-a59b-4e6a-951d-14d8c068e7d2" />
<br>
The data indicates that employees with 2 to 3 years of experience have the highest attrition rate in the organization.

***

7. Does being benched impact an employee’s likelihood of leaving?
```sql
SELECT 
    ever_benched,
    COUNT(*) AS total_employees,
    SUM(resigned) AS total_resigned,
    ROUND(AVG(resigned)*100, 2) AS attrition_rate
FROM 
	employee_dataset
GROUP BY
	ever_benched
ORDER BY
	attrition_rate DESC
;
```
**Key Insight:**

<img width="437" height="68" alt="image" src="https://github.com/user-attachments/assets/1bd7533c-dcc2-423e-b65d-225b34d69d98" />
<br>
The data shows a strong correlation between being benched and the likelihood of leaving the company. Employees who have been benched exhibit a much higher attrition rate compared to those who have not.
