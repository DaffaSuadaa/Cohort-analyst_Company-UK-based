# Cohort-analyst_Company-UK-based
This is a transnational data set which contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based and registered non-store online retail. The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers. (source: [Onlie Retail Dataset](https://www.kaggle.com/datasets/ersany/online-retail-dataset))

# Exploratory Data Analysis
### Monthly Orders and Users
Get the total users who completed the order and total orders per month (Jan 2019-Apr 2022)
 ``` sql
 SELECT 
 DATE_TRUNC(DATE(InvoiceDate), month) as month,
 COUNT(DISTINCT CustomerID) as total_user,
 COUNT(InvoiceNo) as total_order
 FROM
`my-data-analytics-385715.data_online_retail.online_retail_clean`
WHERE DATE(InvoiceDate) BETWEEN "2011-01-01" and "2011-12-30"
GROUP BY month
ORDER BY month;
  ```
**Table results:**

<img src="https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/6bc83613-4dcf-4d22-acae-d2245ac96667" width="500" height="400">

**Insight :** From 1 Jan - 9 Dec 2011 we can see that the total order and total user relatively growing over month.

<img src="https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/6fc062bc-1c06-4116-9d19-34c132e16e9a" width="600" height="350">


### Average Order Value (AOV) and Distinct User

Get average order value and total number of unique users, grouped by month (Jan 2011-Dec 2011)
 ``` sql
SELECT 
DATE_TRUNC(DATE(InvoiceDate), month) as month,
COUNT(DISTINCT CustomerID) as total_user,
ROUND(SUM(UnitPrice)/COUNT(DISTINCT CustomerID),2) AS average_order_value
FROM
`my-data-analytics-385715.data_online_retail.online_retail_clean`
WHERE DATE(InvoiceDate) BETWEEN "2011-01-01" and "2011-12-30"
GROUP BY month
ORDER BY month;
  ```
**Table results:**

<img src="https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/cc766f90-7843-4ea2-8144-6d48b1a7900c" width="500" height="400">

**Insight :** From 1 Jan 2011- 9 Dec 2011 we can see that the number of user increase over the year.

**Note :** Kuvra decreases in December because the data only displays up to the 9th

<img src="https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/dd0a1847-877a-4e01-8e97-20f434d69577" width="600" height="350">


# Cohort Analysis

Monthly retention cohorts (the groups, or cohorts, can be defined based upon the date that a user purchased a product) and then how many of them (%) coming back for the following month in Jan-Dec 2011

```sql
WITH cohort_items AS
(SELECT CustomerID,
MIN(DATE_TRUNC(DATE(InvoiceDate), month)) AS cohort_month,
FROM `my-data-analytics-385715.data_online_retail.online_retail_clean` as o
GROUP BY CustomerID
ORDER BY cohort_month),
user_activity AS
(
  SELECT o.CustomerID,
  DATE_DIFF(DATE(DATE_TRUNC(InvoiceDate, month)), c.cohort_month, month) AS month_number
  FROM `my-data-analytics-385715.data_online_retail.online_retail_clean` AS o
  LEFT JOIN cohort_items AS c
  ON o.CustomerID = c.CustomerID
  GROUP BY CustomerID, month_number
  ORDER BY month_number
),
cohort_size AS
(SELECT cohort_month,
COUNT(cohort_month) AS num_users
FROM cohort_items
GROUP BY cohort_month
ORDER BY cohort_month),
retention_table AS
(SELECT cohort_month,
COUNT(CASE WHEN month_number =1 THEN cohort_month END) AS M1,
COUNT(CASE WHEN month_number =2 THEN cohort_month END) AS M2,
COUNT(CASE WHEN month_number =3 THEN cohort_month END) AS M3,
COUNT(CASE WHEN month_number =4 THEN cohort_month END) AS M4,
COUNT(CASE WHEN month_number =5 THEN cohort_month END) AS M5,
COUNT(CASE WHEN month_number =6 THEN cohort_month END) AS M6,
COUNT(CASE WHEN month_number =7 THEN cohort_month END) AS M7,
COUNT(CASE WHEN month_number =8 THEN cohort_month END) AS M8,
COUNT(CASE WHEN month_number =9 THEN cohort_month END) AS M9,
COUNT(CASE WHEN month_number =10 THEN cohort_month END) AS M10,
COUNT(CASE WHEN month_number =11 THEN cohort_month END) AS M11,
COUNT(CASE WHEN month_number =12 THEN cohort_month END) AS M12
FROM cohort_items AS C
LEFT JOIN user_activity AS U
ON U.CustomerID = C.CustomerID
GROUP BY 1
ORDER BY 1)

SELECT
r.cohort_month AS Month,
c.num_users AS M,
M1, M2, M3, M4, M5, M6, M7, M8, M9, M10, M11, M12
FROM
retention_table AS r
LEFT JOIN cohort_size AS c
ON r.cohort_month =   c.cohort_month
WHERE r.cohort_month is not null and DATE(r.cohort_month) BETWEEN "2011-01-01" and "2011-12-30"
ORDER BY Month;
```
**Table results:**

![WhatsApp Image 2023-05-25 at 15 52 11](https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/071a9ad8-e39d-41ac-8678-9d0ee3bfefa3)

This query is displayed using a Google Sheet connected to BigQuery

![WhatsApp Image 2023-05-25 at 15 52 11](https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/7851848d-9660-45b4-bc40-8625de1fd401)

**Insight :**
* In the first month, retention value is low with an average 19.17%
* Overall during the following month, monthly retention was generally stable at 21.11%
