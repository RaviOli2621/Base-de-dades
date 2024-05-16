### Ex3 Crea el que creguis necessari per auditar la taula d’empleats. Aquesta auditoria pretén dur un control automàtic dels canvis (INSERT, UPDATE, DELETE) que es realitzen en aquesta taula.
~~~mysql
DROP TABLE IF EXISTS auditoria_taules;

CREATE TABLE auditoria_taules
(
	usuari VARCHAR(100),
    data DATETIME,
    taula VARCHAR(64),
    accio VARCHAR(20),
    valors VARCHAR(255)
);

DROP TRIGGER IF EXISTS trg_UpdateEmpleats;

DELIMITER //
CREATE TRIGGER trg_UpdateEmpleats AFTER UPDATE ON empleats FOR EACH ROW
BEGIN 
	INSERT INTO auditoria_taules(usuari, data, taula, accio, valors) 
		VALUES(current_user(), NOW(), "empleats", "DELETE", CONCAT("nom:",IFNULL(OLD.nom,""),"empleat_id:",IFNULL(OLD.empleat_id,""), "cognoms:",IFNULL(OLD.cognoms,""), "email:",IFNULL(OLD.email,""), 
        "telefon:",IFNULL(OLD.telefon,""), "data_contractacio",IFNULL(OLD.data_contractacio,""), "feina_codi:",IFNULL(OLD.feina_codi,""), "salari:",IFNULL(OLD.salari,""), 
        "pct_comissio:",IFNULL(OLD.pct_comissio,""), "id_cap:",IFNULL(OLD.id_cap,""), "departament_id:",IFNULL(OLD.departament_id,"")));
END//
CREATE TRIGGER trg_InsertEmpleats AFTER INSERT ON empleats FOR EACH ROW
BEGIN 
	INSERT INTO auditoria_taules(usuari, data, taula, accio, valors) 
		VALUES(current_user(), NOW(), "empleats", "DELETE", CONCAT("nom:",IFNULL(NEW.nom,""),"empleat_id:",IFNULL(NEW.empleat_id,""), "cognoms:",IFNULL(NEW.cognoms,""), "email:",IFNULL(NEW.email,""), 
        "telefon:",IFNULL(NEW.telefon,""), "data_contractacio",IFNULL(NEW.data_contractacio,""), "feina_codi:",IFNULL(NEW.feina_codi,""), "salari:",IFNULL(NEW.salari,""), 
        "pct_comissio:",IFNULL(NEW.pct_comissio,""), "id_cap:",IFNULL(NEW.id_cap,""), "departament_id:",IFNULL(NEW.departament_id,"")));
END//

CREATE TRIGGER trg_DeleteEmpleats AFTER DELETE ON empleats FOR EACH ROW
BEGIN 
	INSERT INTO auditoria_taules(usuari, data, taula, accio, valors) 
		VALUES(current_user(), NOW(), "empleats", "DELETE", CONCAT("nom:",IFNULL(OLD.nom,""),"empleat_id:",IFNULL(OLD.empleat_id,""), "cognoms:",IFNULL(OLD.cognoms,""), "email:",IFNULL(OLD.email,""), 
        "telefon:",IFNULL(OLD.telefon,""), "data_contractacio",IFNULL(OLD.data_contractacio,""), "feina_codi:",IFNULL(OLD.feina_codi,""), "salari:",IFNULL(OLD.salari,""), 
        "pct_comissio:",IFNULL(OLD.pct_comissio,""), "id_cap:",IFNULL(OLD.id_cap,""), "departament_id:",IFNULL(OLD.departament_id,"")));
END//


DELIMITER ;

UPDATE empleats SET nom = "Juan" WHERE nom = "Paco";
UPDATE empleats SET nom = "Paco" WHERE nom = "Juan";

INSERT INTO empleats (empleat_id, nom, cognoms, email, telefon, data_contractacio, feina_codi, salari, pct_comissio, id_cap, departament_id) 
	VALUES(211, "Eustakio", "asas", "eustakio@empresa.cat", "650.121.1212", "1991-05-21", "SA_REP", 2500, 0.20, 100, 90);

SELECT * FROM auditoria_taules;

DELETE FROM empleats WHERE empleat_id = 211;
~~~
### Ex4 Mitjançant triggers volem dur el control de dades d’entrada d’una taula. Concretament volem dur el control del camp salari de la taula empleats. Aquest salari ha de ser un valor dins del rang marcat pels camps salari_min i salari_max de la taula feines.
~~~mysql
DROP TRIGGER IF EXISTS trg_salari_empleats;
DELIMITER //
CREATE TRIGGER trg_salari_empleats BEFORE UPDATE ON empleats FOR EACH ROW
BEGIN
	IF((NEW.salari < (SELECT salari_min FROM feines WHERE feina_codi = NEW.feina_codi)) OR (NEW.salari > (SELECT salari_max FROM feines WHERE feina_codi = NEW.feina_codi))) THEN
		SET NEW.salari = OLD.salari;
    END IF;
END //

CREATE TRIGGER trg_salari_empleats_Insert BEFORE INSERT ON empleats FOR EACH ROW
BEGIN
	IF((NEW.salari < (SELECT salari_min FROM feines WHERE feina_codi = NEW.feina_codi)) OR (NEW.salari > (SELECT salari_max FROM feines WHERE feina_codi = NEW.feina_codi))) THEN
		SET NEW.salari = (SELECT salari_min FROM feines WHERE feina_codi = NEW.feina_codi);
    END IF;
END //
DELIMITER ;

UPDATE empleats SET salari = 99999 WHERE empleat_id = 101;
INSERT INTO empleats (empleat_id, nom, cognoms, email, telefon, data_contractacio, feina_codi, salari, pct_comissio, id_cap, departament_id) 
	VALUES(212, "Babatunde", "aasas", "babatunde@empresa.cat", "650.121.1212", "1991-05-21", "SA_REP", 10, 0.20, 100, 90);
    
SELECT * FROM empleats;
SELECT * FROM auditoria_taules;
~~~
### Ex5 Afegeix un el camp num_empleats a la taula departaments. Aquest camp simbolitza/modela el número d’empleats que té aquell departament. Implementa mitjançant triggers el manteniment d’aquest camp de manera automàtica
~~~mysql
ALTER TABLE departaments
	ADD num_empleats INT DEFAULT 0;

DROP TRIGGER IF EXISTS trg_num_empleats_insert;
DROP TRIGGER IF EXISTS trg_num_empleats_update;
DROP TRIGGER IF EXISTS trg_num_empleats_delete;

UPDATE departaments d1 -- Inicializar los datos de la tabla
SET d1.num_empleats = (SELECT COUNT(e.empleat_id) FROM empleats e GROUP BY e.departament_id HAVING e.departament_id = d1.departament_id);

DELIMITER //
CREATE TRIGGER trg_num_empleats_update AFTER UPDATE ON empleats FOR EACH ROW
BEGIN 
	IF(OLD.departament_id != NEW.departament_id) THEN
	UPDATE departaments d
	SET d.num_empleats = d.num_empleats + 1
    WHERE d.departament_id = NEW.departament_id;

	UPDATE departaments d
	SET d.num_empleats = d.num_empleats - 1
    WHERE d.departament_id = OLD.departament_id;
    END IF;
END//
CREATE TRIGGER trg_InsertEmpleats_insert AFTER INSERT ON empleats FOR EACH ROW
BEGIN 
	UPDATE departaments d
	SET d.num_empleats = d.num_empleats + 1
    WHERE d.departament_id = NEW.departament_id;
END//

CREATE TRIGGER trg_DeleteEmpleats_delete AFTER DELETE ON empleats FOR EACH ROW
BEGIN 
	UPDATE departaments d
	SET d.num_empleats = d.num_empleats - 1
    WHERE d.departament_id = OLD.departament_id;
END//
DELIMITER ;

~~~
