# Cohort-analyst_Company-UK-based
This is a transnational data set which contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based and registered non-store online retail. The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers. (source: [Onlie Retail Dataset](https://www.kaggle.com/datasets/ersany/online-retail-dataset))

### Monthly Orders and Users

Get the total users who completed the order and total orders per month (Jan 2011-Dec 2011)
# Exploratory Data Analysis
### 1. Monthly Orders and Users
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
 
![WhatsApp Image 2023-05-25 at 15 52 11](https://github.com/DaffaSuadaa/Cohort-analyst_Company-UK-based/assets/134934646/6bc83613-4dcf-4d22-acae-d2245ac96667)

