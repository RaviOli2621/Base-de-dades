# Ex0 MonngoDB
## 1. Obtenir de tots els empleats, el nom, cognoms i salari. Mostrar només 4 registres
~~~javascript
db.empleats.find({},{nom:1,cognoms:1,salari:1,_id:0}).limit(4)
~~~
## 2. Mostra la quantitat de departaments que hi ha
~~~javascript
db.departaments.find({},{}).count()
~~~
## 3. Recupera l’empleat “emplat_id=100”
~~~javascript
db.empleats.find({empleat_id: "100"},{})
~~~
## 4. Recupera els empleats amb el càrrec de “President”
~~~javascript
db.empleats.find({"feina.nom":"President"},{})
~~~
## 5. Recupera els empleats que treballen al departament de “IT”
~~~javascript
db.empleats.find({"departament.nom":"IT"},{})
~~~
## 6. Recupera els empleats que van ser contractats a partir de l’1 de gener de 1985
~~~javascript
db.empleats.find({"data_contractacio":{$gte: new Date (1985-01-01)}},{})
~~~
## 7. Recupera els empleats amb un salari superior a 2000
~~~javascript
db.empleats.find({"salari":{$gt: 2000}},{})
~~~
## 8. Recupera els empleats amb un salari entre 2000 i 6000
~~~javascript
db.empleats.find({"salari":{$gt:2000,$lt:6000}},{})
~~~
## 9. Recupera els empleats que el seu número de telèfon comença per 515
~~~javascript
db.empleats.find({"telefon":/^515/},{})
~~~
## 10.Recupera els empleats que no treballin de Vice President. Utilitza el codi de la feina
"AD_VP"
~~~javascript
db.empleats.find({"feina.codi":"AD_VP"},{})
~~~
## 11.Recupera els empleats que tenen pct_comissio.
~~~javascript
db.empleats.find({"pct_comissio":{"$exists":false}})
~~~
## 12.Recupera els empleats que tenen pct_comissio i hagi treballat o treballin actualment de "Cap de Vendes" . Utilitza el codi de feina "SA_MAN".
~~~javascript

~~~
## 13.Recupera els empleats que han tingut 2 feines. No tinguis en compte la feina actual.
~~~javascript
db.empleats.find({"historial_feines":{$size:2}},{"historial_feines":1})
~~~

# Ex1 MonngoDb
## Ex1
~~~js
db.students.find({"gender":{$eq:"H"}})
~~~
## Ex2
~~~js
db.students.find({"gender":{$eq:"M"}}).count()
~~~
## Ex3
~~~js
db.students.find({"birth_year":{$eq:1993}}).count()
~~~
## Ex4
~~~js
db.students.find({"birth_year":{$eq:1993},"gender":{$eq:"H"}}).count()
~~~
