# TEA-SQL-Challenge-Submission
#Challenge 1
I wrote a query which returns a table sorted by year then month of the sum of transactions from Sales.CustomerTransactions. I believe the negative sales values with no invoice ID should not have been counted so I removed them.
```
/* Challenge 1, Data needed to graph total monthly sales over time.*/
SELECT YEAR(TransactionDate) [Year], MONTH(TransactionDate) [Month], SUM(TransactionAmount) [Montly_Revenue]
FROM Sales.CustomerTransactions 
/* Unsure of why negative sales values with no invoice so remove all with null invoiceIDs */
WHERE InvoiceID IS NOT NULL
GROUP BY YEAR(TransactionDate), MONTH(TransactionDate)
ORDER BY YEAR(TransactionDate), MONTH(TransactionDate) 
```
#Challenge 2
I wrote a query to add the category name to the CustomerTransactions table, sorted by year, quarter, then category. Again I removed the rows with no invoice ID.
By manually calculating between Q1 2016 and Q1 2015 in excel from the resulting table, The biggest growth is in the Computer Store category with a revenue growth of 19.94%.
```
/* Challenge 2, Find fastest growing customer category in Q1 2016 compared to Q1 2015, can manually calculate difference and just display all to save time*/
SELECT YEAR(TransactionDate) [Year], DATEPART(QUARTER, TransactionDate) [Quarter], SUM(TransactionAmount) [Quarterly_Revenue], CustomerCategoryName
FROM Sales.CustomerTransactions
LEFT JOIN Sales.Customers
ON Sales.CustomerTransactions.CustomerID = Sales.Customers.CustomerID
LEFT JOIN Sales.CustomerCategories
ON Sales.Customers.CustomerCategoryID = Sales.CustomerCategories.CustomerCategoryID
/* Unsure of why negative sales values with no invoice so remove all with null invoiceIDs */
WHERE InvoiceID IS NOT NULL
GROUP BY YEAR(TransactionDate), DATEPART(QUARTER, TransactionDate), CustomerCategoryName
ORDER BY YEAR(TransactionDate), DATEPART(QUARTER, TransactionDate), CustomerCategoryName
```
#Challenge 3
This query uses the Purchasing.SupplierTransactions file to count of when OutsandingBalance is above 0, or equal to 0 and find the average transaction amount.
Similar to before, I removed all transactions without a purchaseorderID
```
/* Challenge 3, List suppliers, # of paid invoices, # of invoices outstanding, average invoice amount */
SELECT SupplierName [SupplierName], COUNT(CASE WHEN OutstandingBalance = 0 THEN 1 END) [PaidInvoices], COUNT(CASE WHEN OutstandingBalance > 0 THEN 1 END) [OutstandingInvoices], AVG(TransactionAmount) [AverageInvoiceAmount]
FROM Purchasing.SupplierTransactions
LEFT JOIN Purchasing.Suppliers
ON Purchasing.SupplierTransactions.SupplierID = Purchasing.Suppliers.SupplierID
/* Unsure of why negative transaction amount values with no purchase order so remove all with null purchaseorderIDs */
WHERE PurchaseOrderID IS NOT NULL
GROUP BY SupplierName
```
#Challenge 4
I used the UnitPrice and RecommendedRetailPrice to find the Gross Profit and ordering from least to greatest profit.
By looking at the first entry, the lowest gross profit is 0.33 from '3 kg Courier post bag (White) 300x190x95mm', the greatest profit is 940.01 from 'Air cushion machine (Blue)', and the median can be calculated from dividing the total entries by 2 and going to that entry which is 8.91 from "The Gu" red shirt XML tag t-shirt (White) 5XL/6XL since the total number was odd.
```
/* Challenge 4, Which item in the warehouse has the lowest gross profit, which has the highest, what is the median */
SELECT RecommendedRetailPrice-UnitPrice [GrossProfit], StockItemName
FROM Warehouse.StockItems
ORDER BY RecommendedRetailPrice-UnitPrice
```
#Final Words
Navigating the database took the longest time for me, finding the table which contained the values needed as the biggest database I have worked in before was much smaller. In total I spent around 1 1/2 to complete all 4 challenges and an additional 1/2 hour to document everything.
