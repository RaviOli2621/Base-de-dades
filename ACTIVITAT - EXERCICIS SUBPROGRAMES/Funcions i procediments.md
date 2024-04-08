# Funcions i procediments
[La base de dades utilitzada es rrhh](https://github.com/RaviOli2621/Base-de-dades/blob/main/BasesDeDades/rrhh)
## Funcions
### Ex1
~~~mysql
DELIMITER //
CREATE FUNCTION `spData` (data Date)
RETURNS CHAR(10)
DETERMINISTIC
BEGIN
	
DECLARE dataS CHAR(10);
SET dataS = CONCAT(DAY(data),"-",MONTH(data),"-",YEAR(data));

RETURN dataS;
END//
DELIMITER ;
SELECT spData('1979-10-10') AS data;
~~~
### Ex2
~~~mysql
DELIMITER //
CREATE FUNCTION `spPotencia` (base INT, pot INT)
RETURNS FLOAT
DETERMINISTIC
BEGIN
	DECLARE cont int;
	DECLARE res float;
    SET cont = 0;
	SET res = 1;
    WHILE (pot > cont) DO
		SET res = res * base;
        SET cont = cont +1;
    END WHILE;    
RETURN res;
DELIMITER ;
SELECT spPotencia(2,0) AS num;
~~~
### Ex3
~~~mysql
DELIMITER //
CREATE FUNCTION `spIncrement` (vId INT, vPot INT)
RETURNS DECIMAL(8,2)
NOT DETERMINISTIC READS SQL DATA
BEGIN
		DECLARE vSalari DECIMAL(8,2);
        SET vSalari = ((SELECT salari FROM empleats WHERE empleat_id = vId));
		SET vSalari = vSalari*(vPot+100)/100;
RETURN vSalari;
END//
DELIMITER ;
SELECT spIncrement(101,10) AS Augm;
~~~
### Ex4
~~~mysql
DELIMITER //
CREATE FUNCTION `spPringat` (vDepId INT)
RETURNS INT
NOT DETERMINISTIC READS SQL DATA
BEGIN
RETURN (SELECT empleat_id FROM empleats WHERE departament_id = vDepId ORDER BY salari LIMIT 1);
END//
DELIMITER ;
SELECT spPringat(20) AS empleat_id;
~~~
### Ex5 (Utilitza la funcio del ex4)
~~~mysql
SELECT departament_id, spPringat(departament_id) AS empleat_id FROM departaments HAVING empleat_id ORDER BY departament_id;
~~~
### Ex6
~~~mysql
DELIMITER //
CREATE FUNCTION `spCategoria` (vEmplId INT)
RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN
	DECLARE vDataIn date;
    SET vDataIn = (SELECT data_contractacio FROM empleats WHERE empleat_id = vEmplId);
RETURN TIMESTAMPDIFF(YEAR, vDataIn, curdate());
END//
DELIMITER ;
SELECT spCategoria(101) AS anys;
~~~

## Procedures

### Ex1
~~~mysql
DELIMITER //
CREATE PROCEDURE spDataUsuari()
BEGIN

	SELECT Now();
    
END //
DELIMITER ;

CALL spDataUsuari();
~~~

### Ex2
~~~mysql
DELIMITER //
CREATE FUNCTION spEmpleatEx (pId INT) RETURNS BOOLEAN
NOT DETERMINISTIC READS SQL DATA
BEGIN
	DECLARE vRetorn BOOLEAN;
    SET vRetorn = 0;
    IF(SELECT empleat_id FROM empleats WHERE empleat_id = pId) THEN SET vRetorn = 1;
    END IF;
    RETURN vRetorn;
END //
CREATE PROCEDURE ex2(IN pId1 INT,IN pId2 INT)
BEGIN
	
    -- Creamos variables para los sueldos 
	DECLARE  s1 decimal(8,2);
    DECLARE  s2 decimal(8,2);
    -- Si los empleados existen...
    IF((SELECT spEmpleatEx(pId1) = 1) AND (SELECT spEmpleatEx(pId2) = 1)) THEN
    -- Guardar sueldos
    SET s1 = (SELECT salari FROM empleats WHERE empleat_id = pId1 LIMIT 1);
    SET s2 = (SELECT salari FROM empleats WHERE empleat_id = pId2 LIMIT 1);
    -- Canviar suledos
    UPDATE empleats
		SET salari = s2
	WHERE empleat_id = pId1;
    
    UPDATE empleats
		SET salari = s1
	WHERE empleat_id = pId2;
    END IF;
END //
DELIMITER ;

CALL ex2(100,101);
SELECT empleat_id, salari FROM empleats;
~~~

### Ex3
~~~mysql
DELIMITER //
CREATE PROCEDURE ex3(IN pId1 INT,IN pId2 INT)
BEGIN
    -- Creamos variables para los sueldos 
	DECLARE pDep1 decimal(8,2);
    DECLARE pDep2 decimal(8,2);
    -- Si los empleados existen...
    IF((SELECT spEmpleatEx(pId1) = 1) AND (SELECT spEmpleatEx(pId2) = 1)) THEN
    -- Guardar sueldos
    SET pDep1 = (SELECT departament_id FROM empleats WHERE empleat_id = pId1 LIMIT 1);
    SET pDep2 = (SELECT departament_id FROM empleats WHERE empleat_id = pId2 LIMIT 1);
    -- Canviar suledos
    UPDATE empleats
		SET departament_id = pDep2
	WHERE empleat_id = pId1;
    
    UPDATE empleats
		SET departament_id = pDep1
	WHERE empleat_id = pId2;
    END IF;
END //
DELIMITER ;

CALL ex3(100,103);
~~~
### Ex4
~~~mysql
DELIMITER //
CREATE PROCEDURE spMouEmpl(IN dDep1 INT, IN dDep2 INT)
BEGIN
	IF dDep1 IS NOT NULL THEN
	UPDATE empleats
		SET departament_id = dDep1 
	WHERE departament_id = dDep2 ;
    END IF;
END //
DELIMITER ;

CALL spMouEmpl(90,60);
~~~

### Ex5
~~~mysql
DELIMITER //
CREATE PROCEDURE spSelectEmpleats()
BEGIN
	SELECT e.empleat_id, e.nom, d.nom AS departament, l.ciutat
		FROM empleats e
	INNER JOIN departaments d ON d.departament_id = e.departament_id
	INNER JOIN localitzacions l ON l.localitzacio_id = d.localitzacio_id
    ORDER BY e.empleat_id;
END //
DELIMITER ;
~~~

### Ex6
~~~mysql
CREATE PROCEDURE spSelectEmpleats2()
BEGIN
	SELECT *
		FROM empleats 
		ORDER BY empleat_id;
END //
DELIMITER ;
~~~

### Ex7
~~~mysql
DELIMITER //
CREATE PROCEDURE spRegistre(IN prId INT)
BEGIN
	CREATE TABLE IF NOT EXISTS registre
    (
		id INT UNSIGNED AUTO_INCREMENT,
        data DATETIME, 
		CONSTRAINT pk_registre PRIMARY KEY (id,data)
    ); 
    INSERT INTO registre(id, data) VALUE(prId, NOW());
    SELECT * FROM registre;
END //
DELIMITER ;

CALL spRegistre(101);
~~~
