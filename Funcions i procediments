# Funcions i procediments
## Funcions
### Ex1
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

### Ex2
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

### Ex3
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

### Ex4
DELIMITER //
CREATE FUNCTION `spPringat` (vDepId INT)
RETURNS INT
NOT DETERMINISTIC READS SQL DATA
BEGIN
RETURN (SELECT empleat_id FROM empleats WHERE departament_id = vDepId ORDER BY salari LIMIT 1);
END//
DELIMITER ;
SELECT spPringat(20) AS empleat_id;

### Ex5 (Utilitza la funcio del ex4)
SELECT departament_id, spPringat(departament_id) AS empleat_id FROM departaments HAVING empleat_id ORDER BY departament_id;

### Ex6
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





