###  <img src="https://lh3.googleusercontent.com/gctl5Kr4FgPAjPQKpdojG0TjI2VJE6-r1voLBbR0MnjP5IpLUS4tWffP2hdfwxIqCoA=w80" alt="drawing" width="25"/>  Этот репозиторий содержит образцы SQL-запросов по задачам с сайта [sql-ex.ru](https://sql-ex.ru/)



| Название БД       | Описание                          |Темы|
| :-------------:|:------------------------:|:-----:|
| [Computer Company](https://github.com/calculatedmediocrity/SQL-Ex.ru/blob/78874c8eab94c736f35a7bacde56a79d3ef181e2/DB%20Computer%20Company/DB_Schema.md)|Содержит информацию о производителях ПК, ноутбуков, принтеров |Selection, single row functions, Joins, Subqueries|

1. Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd

```sql
SELECT model, speed, hd FROM PC
WHERE price < 500; 
```

2. Найдите производителей принтеров. Вывести: maker

```sql
SELECT DISTINCT maker FROM Product
WHERE type = 'printer';
```

3. Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.

```sql
SELECT model, ram, screen 
FROM laptop
WHERE price > 1000;
```

4. Найдите все записи таблицы Printer для цветных принтеров.

```sql
SELECT * 
FROM Printer
WHERE color = 'y'
```

5. Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.

```sql
SELECT model, speed, hd
FROM PC
WHERE (cd = '12x' OR cd = '24x') AND price < 600;
```

6. Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.

```sql
SELECT DISTINCT Product.maker, Laptop.speed
FROM Product
INNER JOIN Laptop 
    ON Product.model = Laptop.model
WHERE Laptop.hd >= 10;
```

7. Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).

```sql
SELECT Product.model, price FROM Product 
INNER JOIN PC 
    ON Product.model = PC.model
WHERE Product.maker = 'B'

UNION

SELECT Product.model, price FROM Product 
INNER JOIN Laptop
    ON Product.model = Laptop.model
WHERE Product.maker = 'B'

UNION

SELECT Product.model, price FROM Product 
INNER JOIN Printer
    ON Product.model = Printer.model
WHERE Product.maker = 'B';
```

8. Найдите производителя, выпускающего ПК, но не ПК-блокноты.

```sql
SELECT maker FROM Product
WHERE type = 'PC' 
EXCEPT
SELECT maker FROM Product
WHERE type = 'Laptop';
```

9. Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker

```sql
SELECT DISTINCT maker FROM Product
INNER JOIN PC 
    ON Product.model = PC.model
WHERE speed >= 450;
```

 10. Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price

```sql
SELECT Product.model, price FROM Product
INNER JOIN Printer
    ON Product.model = Printer.model
WHERE price IN 
    (SELECT MAX(price) FROM Printer);
```

11. Найдите среднюю скорость ПК.
```sql
SELECT AVG(speed) FROM PC;
```

12. Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT AVG(speed) FROM Laptop
WHERE price IN 
    (SELECT price FROM Laptop
    WHERE price > 1000);
```

13. Найдите среднюю скорость ПК, выпущенных производителем A.
```sql
SELECT AVG(speed) FROM PC
INNER JOIN Product 
    ON Product.model = PC.model
WHERE maker IN 
    (SELECT maker FROM Product
    WHERE maker = 'A');
```

14. Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.
```sql
SELECT Ships.class, name, country 
FROM Classes INNER JOIN Ships
    ON Classes.class = Ships.class
WHERE numGuns >= 10;
```

15. Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD
```sql
SELECT hd
FROM PC
GROUP BY hd
HAVING COUNT(hd) > 1;
```

16. Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i).
Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
```sql
SELECT DISTINCT A.model AS model_1, B.model AS model_2, A.speed, A.ram
FROM PC AS A, PC B
WHERE A.speed = B.speed AND A.ram = B.ram AND A.model > B.model
```

17.Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.
Вывести: type, model, speed
```sql
SELECT DISTINCT type, Product.model, speed
FROM Laptop
INNER JOIN Product
    ON Laptop.model = Product.model
WHERE type = 'Laptop' AND speed < 
    (SELECT MIN(speed)
    FROM PC);
```

18. Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price
```sql
SELECT DISTINCT maker, price FROM Printer
INNER JOIN Product
    ON Product.model = Printer.model
WHERE color = 'y' AND price = 
    (SELECT MIN(price)
    FROM Printer
    WHERE color = 'y');
```

19. Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.
Вывести: maker, средний размер экрана.
```sql
SELECT maker, AVG(screen)
FROM Product INNER JOIN Laptop
    ON Product.model = Laptop.model
WHERE Product.model IN 
    (SELECT model FROM Laptop)
GROUP BY maker;
```

20. Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.
```sql
SELECT DISTINCT maker, COUNT(model) AS count
FROM Product
WHERE type = 'PC'
GROUP BY maker
HAVING COUNT(model) >= 3;
```

21. Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC.
Вывести: maker, максимальная цена.
```sql
SELECT DISTINCT maker, MAX(price) AS max_price FROM Product
INNER JOIN PC
    ON Product.model = PC.model
GROUP BY maker;
```

22. Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.
```sql
SELECT speed, AVG(price) AS average_price FROM PC
WHERE speed > 600
GROUP BY speed;
```

23. Найдите производителей, которые производили бы как ПК со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker
```sql
SELECT DISTINCT maker FROM Product
INNER JOIN PC
    ON Product.model = PC.model
WHERE speed >= 750;

INTERSECT 

SELECT DISTINCT maker FROM Product
INNER JOIN Laptop
    ON Product.model = Laptop.model
WHERE speed >= 750;
```

24. Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции.
```sql
SELECT T1.model FROM 
    (SELECT model, price FROM PC
    UNION
    SELECT model, price FROM Laptop
    UNION 
    SELECT model, price FROM Printer) AS T1
WHERE price = 
    (SELECT MAX(price) FROM
        (SELECT price FROM PC
        UNION
        SELECT price FROM Laptop
        UNION 
        SELECT price FROM Printer) AS T2);
```

25. Найдите производителей принтеров, которые производят ПК с наименьшим объемом RAM и с самым быстрым процессором среди всех ПК, имеющих наименьший объем RAM.
Вывести: Maker
```sql
SELECT DISTINCT maker
FROM Product
WHERE model IN 
    (SELECT model FROM PC
    WHERE speed IN 
        (SELECT MAX(speed) FROM PC
        WHERE ram IN 
            (SELECT MIN(ram) FROM PC))
        AND ram IN 
            (SELECT MIN(ram) FROM PC))
        AND maker IN 
            (SELECT maker
            FROM Product
            WHERE type = 'Printer');
```
            
 26. Найдите среднюю цену ПК и ПК-блокнотов, выпущенных производителем A (латинская буква). Вывести: одна общая средняя цена. 
 ```sql
 SELECT AVG(price) AS AVG_price
FROM 
    (SELECT price, type
    FROM PC INNER JOIN Product
        ON PC.model = Product.model
    WHERE maker = 'A'

    UNION ALL

    SELECT price, type
    FROM Laptop INNER JOIN Product
        ON Laptop.model = Product.model
    WHERE maker = 'A'
    ) AS Table1;
```

27. Найдите средний размер диска ПК каждого из тех производителей, которые выпускают и принтеры. Вывести: maker, средний размер HD.
```sql
SELECT maker, AVG(hd) AS AVG_hd
FROM PC
INNER JOIN Product
    ON Product.model = PC.model
WHERE maker IN 
    (SELECT maker
    FROM Product
    WHERE type = 'Printer')
GROUP BY  maker;
```

28. Используя таблицу Product, определить количество производителей, выпускающих по одной модели.
```sql
SELECT COUNT(*) AS count
FROM 
    (SELECT maker,
         COUNT(model) AS count
    FROM Product
    GROUP BY  maker
    HAVING COUNT(model) = 1) AS T1;
```    
