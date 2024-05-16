### Ex1
~~~mysql
CREATE TABLE IF NOT EXISTS logs_usuaris
(
	usuari VARCHAR(100),
    data DATETIME,
    taula VARCHAR(64),
    accio VARCHAR(20),
    valor_pk VARCHAR(200),
    error INT
);

CREATE TABLE IF NOT EXISTS empleat_copia
(
	id_empleat VARCHAR(10),
	vNom VARCHAR(20),
	vCognom VARCHAR(20)
);

DROP PROCEDURE IF EXISTS spCopiaEmpleat;

DELIMITER //

CREATE PROCEDURE spCopiaEmpleat(pEmpleat_id INT)
BEGIN
	DECLARE vNom VARCHAR(20);
    DECLARE vCognoms VARCHAR(25);
    DECLARE vEmpleatId INT;
	DECLARE tmp_nom VARCHAR(20);
    DECLARE tmp_cognoms VARCHAR(25);
    DECLARE tmp_id_empleat INT;
    DECLARE fi_cursor BOOLEAN DEFAULT false;
    DECLARE cEmpleats CURSOR FOR
		SELECT empleat_id, nom, cognoms FROM empleats;
	DECLARE CONTINUE HANDLER FOR NOT FOUND -- Para acabar el bucle
		BEGIN 
			SET fi_cursor = true;
		END;
	DECLARE CONTINUE HANDLER FOR SQLSTATE "23000" -- Error en la pk
		BEGIN
			INSERT INTO logs_usuaris(usuari, data, taula, accio, valor_pk, error) VALUES (tmp_id_empleat, curdate(), "empleat_copia", "COPIA_EMPL", "el codi empleat que hem intentat traspassar", 2);
			SELECT * FROM logs_usuaris;
        END;    
	-- ----------------------------------------------------------
	OPEN cEmpleats;
    
    FETCH cEmpleats INTO tmp_id_empleat, tmp_nom, tmp_cognoms;
    
    WHILE fi_cursor = false AND NOT (vEmpleatId = pEmpleat_id) DO
        FETCH cEmpleats INTO vEmpleatId,vNom,vCognoms;
	END WHILE;
	IF(fi_cursor = true) THEN 
		INSERT INTO logs_usuaris(usuari, data, taula, accio, valor_pk, error) VALUES (tmp_id_empleat, curdate(), "empleat_copia", "COPIA_EMPL", "el codi empleat que hem intentat traspassar", 1);
		SELECT * FROM logs_usuaris;
    END IF;
   
    CLOSE cEmpleats;
        
END //
DELIMITER ;

CALL spCopiaEmpleat(1011);
~~~
