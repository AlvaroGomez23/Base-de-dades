<h1>Excercicis UF3</h1>

# Enunciats de funcions
Exercici 1 - Fes una funció anomenada spData, tal que donada una data en format
MySQL ( AAAA-MM-DD ) ens retorni una cadena de caràcters en format DD-MM-AAAA
Exemple : SELECT spData('1988-12-01') => 01-12-1988
```sql
DELIMITER //

CREATE FUNCTION spData (data DATE) RETURNS CHAR(10)
DETERMINISTIC
BEGIN
	
	DECLARE sData CHAR(10);
    
    SET sData = DATE_FORMAT(data, "%d-%m-%Y");
    RETURN sData;

END
//
```



Exercici 2 - Fes una funció anomenada spPotencia, tal que donada una base i un
exponent, ens calculi la seva potència. Intenta no utilitzar la funció POW.
Exemple : SELECT spPotencia(2,3) => 8



Exercici 3 - Fes una funció anomenada spIncrement que donat un codi d’empleat i un
% de increment, ens calculi el salari sumant aquest percentatge.
Per exemple, suposem que l’ empleat amb id_empleat = 124 té un salari de 1000
Exemple: SELECT spIncrement(124,10) obtindriem -> 1100
```sql
CREATE FUNCTION spIncrement (pEmpleat INT, pIncrement FLOAT) RETURNS FLOAT
NOT DETERMINISTIC READS SQL DATA
BEGIN


	DECLARE vRetorn FLOAT;

	SELECT salari*(pIncrement+100)/100 INTO vRetorn
		FROM empleats
	WHERE empleat_id = pEmpleat;
   
    RETURN vRetorn;
END
//
```



Exercici 4 - Fes una funció anomenada spPringat, tal que li passem un codi de
departament, i ens torni el codi d’empleat que guanya menys d’aquell departament.
```sql
CREATE FUNCTION spPringat (pDepartamentId INT) RETURNS INT 
NOT DETERMINISTIC READS SQL DATA
BEGIN 

	DECLARE vRetorn INT;
    
    SELECT empleat_id INTO vRetorn
		FROM empleats
	WHERE departament_id = pDepartamentId
    ORDER BY empleat_id ASC
    LIMIT 1;
    
    RETURN vRetorn;
    
END
//
```



Exercici 5 - Utilitzant la funció spPringat fes una consulta per obtenir de cada
departament, l’empleat pringat. Mostra el codi i nom del departament, i el codi d’empleat.



Exercici 6 - Fes una funció anomenada spCategoria, tal que donat un codi d’empleat,
ens digui en quina categoria professional està. El criteri que volem seguir per determinar
la categoria professional és en funció dels anys que porta treballant a l’empresa:
Entre 0 i 1 anys -> Auxiliar
Entre 2 i 10 anys -> Oficial de Segona
Entre 11 i 20 Anys -> Oficial de Primera
Més de 20 anys -> Que es jubili!
```sql
DROP FUNCTION IF EXISTS spCategoria;
DELIMITER //
CREATE FUNCTION spCategoria(pEmpleatId INT) RETURNS VARCHAR(20)
NOT DETERMINISTIC READS SQL DATA
BEGIN
	DECLARE vRetorn VARCHAR(20);
    DECLARE anys INT;

	SELECT YEAR(CURDATE()) - YEAR(data_contractacio) INTO anys
		FROM empleats
	WHERE empleat_id = pEmpleatid;
    
    SET vRetorn = CASE
    WHEN anys BETWEEN 0 AND 1 THEN "Auxiliar"
    WHEN anys BETWEEN 2 AND 10 THEN "Oficial de segona"
    WHEN anys BETWEEN 11 AND 20 THEN "Oficial de primera"
    ELSE "Que es jubili"
    END;
    
    RETURN vRetorn;
END
//
```

Exercici 7 - Fes una consulta utilitzant la funció anterior perquè mostri mostri de cada
empleat, el codi d’empleat, el nom, els anys treballats i la categoria professional a la que
pertany.

Exercici 8 - Fes una funció anomenada spEdat, tal que donada una data per paràmetre
ens retorni l'edat d'una persona. Les dates posteriors a la data d'avui han de retornar 0.



Exercici 9 - Fes una funció que ens retorni el número de directors (caps) diferents tenim.



Exercici 10 - Quina instrucció utilitzarem si volem veure el contingut de la funció
spPringat?



# Enunciats de procediments

Exercici 1 - Fes un procediment que permeti obtenir la data i hora del sistema i l’usuari
actual.
```sql

DROP PROCEDURE IF EXISTS sp_obtenirDades;
DELIMITER //
CREATE PROCEDURE sp_obtenirDades ()
BEGIN
	SELECT USER() AS usuari, NOW() AS "data i hora";
END
//
```



Exercici 2 - Fes un procediment que intercanvii el sou de dos empleats passats per
paràmetre.

Funció spEmpleatExisteix:
```sql
CREATE FUNCTION spEmpleatExisteix (pEmpleatId INT) RETURNS BOOLEAN
BEGIN 
	
    DECLARE vRetorn BOOLEAN;
    
    IF (SELECT empleat_id
				FROM empleats
			WHERE empleat_id = pEmpleatId) THEN 
		SET vRetorn = 1;
	END IF;
    
    RETURN vRetorn;
END 
//
```

```sql
DROP PROCEDURE IF EXISTS spCambiarSou;
DELIMITER //
CREATE PROCEDURE spCambiarSou (IN pEmpleatId1 INT, IN pEmpleatId2 INT)
BEGIN


DECLARE vSalariTemp DECIMAL (8,2);

IF spEmpleatExisteix (pEmpleatId1) = 1
	AND spEmpleatExisteix (pempleatId2) = 1 THEN

SELECT salari INTO vSalariTemp
	FROM empleats
WHERE empleat_id = pEmpleatId1;

UPDATE empleats
	SET salari = (SELECT salari
					FROM emeplats
					WHERE empleat_id = pEmpleatId2)
WHERE empleat_id = pEmpleatId1;

UPDATE empleats
	SET salari = vSalariTemp
WHERE empleat_id = pEmpleatId2;

END IF;

END
//
```


Exercici 3 - Fes un procediment que donat dos Ids d'empleat assigni el codi de
departament del primer en el segon.

```sql

CREATE PROCEDURE spCambiarDep (IN pEmpleatId1 INT, IN pEmpleatId2 INT)
BEGIN

DECLARE vDepEmp1 INT;

-- Comprova que els 22 empleats existeixin
IF spEmpleatExisteix (pEmpleatId1) = 1
	AND spEmpleatExisteix (pEmpleatId2) = 1 THEN
-- Obtenir departament_id de pEmpleatId1
SELECT departament_id INTO vDepEmp1
	FROM empleats
WHERE empleat_id = pEmpleatId1;

-- Modificar departament_id de pEmpleatId2
UPDATE empleats 
	SET departament_id = vDepEmp1
WHERE empleat_id = pEmpleatId2;

END IF; 
END//

```



Exercici 4 - Fes un procediment que donat dos codis de departament assigni tots els
empleats del segon en el primer. Un cop executat el procediment el departament que
correspont en el segon paràmetre ha de quedar desert/sense cap empleat.



Exercici 5 - Fes un procediment per mostrar un llistat dels empleats. Volem veure el
id_empleat, nom_empleat, nom_departament i el nom de la localització del departament

```sql
DELIMITER //
DROP PROCEDURE IF EXISTS spMostrarInfoEmpleat;

CREATE PROCEDURE spMostrarInfoEmpleat ()
BEGIN

SELECT e.empleat_id, e.nom, d.nom, l.ciutat
	FROM empleats e
	INNER JOIN departaments d ON d.departament_id = e.departament_id
    INNER JOIN localitzacions l ON l.localitzacio_id = d.localitzacio_id;
END
//

```



Exercici 6 - Fes un procediment que donat un codi d’empleat, ens doni la informació de
l’empleat ( agafa la informació que creguis rellevant)



Exercici 7 - Volem fer un registre dels usuaris que entren al nostre sistema. Per fer-ho
primer caldrà crear una taula amb dos camps, un per guardar l’usuari i l’altre per guardar
la data i hora de l’accés.
```sql
CREATE TABLE registre_usuaris (
	usuari VARCHAR(30)
	data_access DATE
);
```


Exercici 8 - A continuació feu un procediment sense arguments, de manera que cada
vegada que el crideu, insereixi en aquesta taula l’usuari actual i la data i hora en que s’ha
executat el procediment.
```sql
DROP PROCEDURE IF EXISTS spRegistrarUsuari;
DELIMITER //
CREATE PROCEDURE spRegistrarUsuari()
BEGIN 
	INSERT INTO registre_usuaris(usuari, data_access)
		VALUES(CURRENT_USER(), NOW());
        
END
//
```


Exercici 9 - Fes un procediment que ens permeti afegir un nou departament però amb la
següent particularitat: En cas que la localització no existeixi a la taula localitzacions, ens
posarà un NULL en el camp id_localtizacio de la taula departaments. Al procediment li
hem de passar el codi de departament, el nom del departament i el codi de la localització.



Exercici 10 - Fes un procediment que donat un codi d’empleat, ens posi en paràmetres
de sortida el nom i el cognom. Indica com ho pots fer per comprovar si el procediment et
funciona.
```sql
DROP PROCEDURE IF EXISTS spDadesEmpleat;
DELIMITER //
CREATE PROCEDURE spDadesEmpleat(IN pEmpleatId INT, OUT pEmpleatNom VARCHAR(20), OUT pEmpleatCognoms VARCHAR(40))
BEGIN
	SELECT nom, cognoms INTO pEmpleatNom, pEmpleatCognoms
		FROM empleats
	WHERE empleat_id = pEmpleatId;
END
//
```



Exercici 11 - Fes un procediment que ens permeti modificar el nom i cognom d’un
empleat.



Exercici 12 - Crea una taula d’auditoria anomenada logs_usuaris. Aquesta taula la
utilitzarem per monitoritzar algunes de les accions que fan els usuaris sobre les dades,
per exemple si actualitzen dades, eliminen registres.



La taula ha de tenir els següents camps:

usuari VARCHAR(100) Usuari que ha realitzat l’acció
data DATETIME Data-Hora en que s’ha realitzat l’acció
taula VARCHAR(50) Taula sobre la que es realitza l’acció
accio VARCHAR(20) “ELIMINAR”,”AFEGIR”,”MODIFICAR”,”INSERIR”
valor_pk VARCHAR(200) valor que identifica el registre que ha patit
l’acció


Fes un procediment amb nom spRegistrarLog que rebrà com a paràmetres el nom de la
taula, l’acció i el valor_pk.
Aquest procediment només cal que insereixi un registre en la taula logs_usuaris amb les
dades rebudes, tenint en compte l’usuari actual i la data-hora del sistema.

# Enunciats de triggers

Exercici 1: Omplir la següent taula indicant en cada cas si es pot accedir als
registres NEW i OLD des del trigger corresponent i, en cas afirmatiu, quin tipus
d'operació és pot realitzar (L(llegir) o M(modificar)).


Exercici 2: A quina taula o taules de la base de dades INFORMATION_SCHEMA de
MySQL es poden consultar els triggers que hi ha a la base dades?


Exercici 4: Validació de dades d’entrada
Mitjançant triggers volem dur el control de dades d’entrada d’una taula.
Concretament volem dur el control del camp salari de la taula empleats. Aquest
salari ha de ser un valor dins del rang marcat pels camps salari_min i
salari_max de la taula feines.
En definitiva, volem controlar que el salari dels empleats estigui dins dels rangs de
salaris marcats per el tipus de feina que fa l’empleat.



Exercici 5: Manteniment del camp num_empleats
Afegeix un el camp num_empleats a la taula departaments. Aquest camp
simbolitza/modela el número d’empleats que té aquell departament.
Implementa mitjançant triggers el manteniment d’aquest camp de manera
automàtica.
Per exemple si s’afegeix un nou empleat del departament amb codi 10 cal
augmentar en 1 aquest camp del departament_id = 10

