---
title: '[우아한테크코스] SQL 퀴즈 및 정답 (SQL Quizzes and Answers)'
author: da-nyee
date: 2021-04-03 23:40:10 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, sql, quizzes, answers]
---

## 1.
### Quiz
200개 이상 팔린 상품명과 그 수량을 수량 기준 내림차순으로 보여주세요.

### Answer
```sql
SELECT Products.ProductID, ProductName, SUM(Quantity)
FROM Products
JOIN OrderDetails ON Products.ProductID = OrderDetails.ProductID
GROUP BY ProductName
HAVING SUM(Quantity) >= 200
ORDER BY SUM(Quantity) DESC;
```

## 2.
### Quiz
많이 주문한 순으로 고객 리스트(ID, 고객명)를 구해주세요. (고객별 구매한 물품 총 갯수)

### Answer
```sql
SELECT C.CustomerID, C.CustomerName, O.Sum_Quantity
FROM Customers AS C
LEFT JOIN (
	SELECT CustomerID, SUM(Quantity) AS Sum_Quantity
	FROM Orders
	JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
	GROUP BY Orders.CustomerID) AS O ON C.CustomerID = O.CustomerID
ORDER BY O.Sum_Quantity DESC;
```

## 3.
### Quiz
많은 돈을 지출한 순으로 고객 리스트를 구해주세요.

### Answer
```sql
SELECT C.CustomerID, C.CustomerName, OODP.Result
FROM Customers AS C
LEFT JOIN (
	SELECT O.CustomerID, sum(ODP.TotalPrice) AS Result
	FROM Orders AS O
	JOIN (
		SELECT OD.OrderID AS OrderID, SUM(P.Price * OD.Quantity) AS TotalPrice
		FROM OrderDetails AS OD
		JOIN Products AS P ON OD.ProductID = P.ProductID
		GROUP BY OD.OrderID) AS ODP ON O.OrderID = ODP.OrderID
	GROUP BY O.CustomerID) AS OODP ON C.CustomerID = OODP.CustomerID
ORDER BY OODP.Result DESC;
```