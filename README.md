# SQL_Additional_Practice
## (The initial practice (1-16 exercises) is in Ksendzov_SQL repository)
```sql

17. Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.
Вывести: type, model, speed

SELECT DISTINCT product.type, laptop.model, laptop.speed
FROM laptop
JOIN product ON laptop.model = product.model
WHERE speed < ALL (SELECT speed FROM pc)

18. Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price

SELECT DISTINCT p.maker, pr.price
FROM Product p  INNER JOIN  Printer pr ON p.model = pr.model
WHERE pr.price =
  ( SELECT MIN(price)
    FROM Printer Pr_in
    WHERE pr.type = Pr_in.type )
AND color = 'y'

19. Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.
Вывести: maker, средний размер экрана.

SELECT maker, AVG(laptop.Screen)
FROM Product
JOIN Laptop on laptop.model = product.model
GROUP BY Maker

20. Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.

Select maker, COUNT(model) as Modell
FROM Product
WHERE type = 'PC'
GROUP BY maker
HAVING COUNT(DISTINCT model) >= 3

21. Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC.
Вывести: maker, максимальная цена.

SELECT maker, MAX(PC.price)
FROM Product
JOIN PC on PC.model = Product.model
GROUP BY Maker

22. Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.

SELECT speed, AVG(Price)
FROM PC
WHERE speed > 600
GROUP BY speed

23. Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker

SELECT Maker
FROM Product
JOIN PC on PC.model = Product.model
WHERE PC.speed >=750
INTERSECT
SELECT Maker
FROM Product
JOIN Laptop ON Laptop.model = Product.model
WHERE Laptop.speed >=750

24. Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции.

WITH model_count AS (
SELECT model, price AS max_price FROM PC
UNION ALL
SELECT model, price AS max_price FROM Laptop
UNION ALL
SELECT model, price AS max_price FROM Printer)
SELECT DISTINCT model FROM model_count WHERE max_price = (SELECT MAX(max_price) FROM model_count)


