### Ex1
~~~mysql
WITH RECURSIVE Time_Series2 (o) AS
(
SELECT "2021-01-01" as o
UNION ALL
SELECT DATE_ADD(o,INTERVAL 1 DAY)
FROM Time_Series2
WHERE o < "2021-01-31"
)
SELECT *
FROM Time_Series2;
~~~
### Ex2
~~~mysql
WITH RECURSIVE Time_Series1 (o) AS
(
SELECT concat(date_format(curdate(), "%Y"),"-01-01") as o
UNION ALL
SELECT DATE_ADD(o,INTERVAL 1 DAY)
FROM Time_Series1
WHERE o < concat(date_format(curdate(), "%Y"),"-01-31")
)
SELECT *
FROM Time_Series1;
~~~
### Ex3
~~~mysql
WITH RECURSIVE em_path AS 
(
	SELECT e0.empleat_id, e0.cognoms, e0.id_cap, 0 nivell
		FROM empleats e0
    WHERE e0.empleat_id = 149
    UNION ALL
    SELECT en.empleat_id, en.nom, en.cognoms, en.id_cap, nivell+1
    FROM empleats en
    INNER JOIN emp_path ep on ep.empleat_id = en.id_cap
)
SELECT *
	FROM emp_path
ORDER BY nivell;
~~~
### Ex4
~~~mysql
WITH RECURSIVE paco(o) AS
(
	SELECT ("1994-01-01") AS o
    UNION ALL
    SELECT DATE_ADD(o, INTERVAL 1 MONTH)
    FROM paco
    WHERE o < "1994-12-01"
)
SELECT MONTH(o) AS month, COUNT(e.empleat_id)
FROM paco p
LEFT JOIN empleats e ON LEFT(e.data_contractacio,7) = LEFT(p.o,7)
GROUP BY month;
~~~
### Ex5
~~~mysql
WITH paco as
(
	SELECT *
    FROM regions
)
SELECT pa.nom, COUNT(p.nom)
FROM paisos p
INNER JOIN paco pa ON pa.regio_id = p.regio_id
GROUP BY pa.nom
ORDER BY pa.nom;
~~~
### Ex6
~~~mysql
WITH depCount as
(
	SELECT departament_id, COUNT(empleat_id) AS count
    FROM empleats
    GROUP BY departament_id
)
SELECT e.nom, d.count
FROM empleats e
INNER JOIN depCount d ON e.departament_id = d.departament_id;


WITH paco AS (SELECT departament_id, DENSE_RANK() OVER (ORDER BY -COUNT(empleat_id)) AS hola, COUNT(empleat_id) 
FROM empleats
GROUP BY departament_id)
SELECT *
FROM paco
WHERE hola = 3;
~~~
