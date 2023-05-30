# Cohort-analyst_Company-UK-based
This is a transnational data set which contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based and registered non-store online retail. The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers. (source: [Onlie Retail Dataset](https://www.kaggle.com/datasets/ersany/online-retail-dataset))

### Monthly Orders and Users

Get the total users who completed the order and total orders per month (Jan 2011-Dec 2011)
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

<img src="https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/6fc062bc-1c06-4116-9d19-34c132e16e9a" width="600" height="400">


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
<img src="https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/cc766f90-7843-4ea2-8144-6d48b1a7900c" width="500" height="400">

**Insight :** From 1 Jan 2011- 9 Dec 2011 we can see that the number of user increase over the year.

**Note :** Kuvra decreases in December because the data only displays up to the 9th

<img src="https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/dd0a1847-877a-4e01-8e97-20f434d69577" width="500" height="400">

