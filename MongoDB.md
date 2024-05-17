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
db.empleats.find({$not:{"feina.codi":"AD_VP"}},{})
~~~
## 11.Recupera els empleats que tenen pct_comissio.
~~~javascript
db.empleats.find({"pct_comissio":{"$exists":false}})
~~~
## 12.Recupera els empleats que tenen pct_comissio i hagi treballat o treballin actualment de "Cap de Vendes" . Utilitza el codi de feina "SA_MAN".
~~~javascript
db.empleats.find({"pct_comissio":{}})
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
## Ex5
~~~js
db.students.find({"birth_year":{$gt:1990}})
~~~
## Ex6
~~~js
db.students.find({"birth_year":{$lte:1990}})
~~~
## Ex7
~~~js
db.students.find({$and:[{"birth_year":{$gte:1990}},{"birth_year":{$lte:2000}}]})
~~~
## Ex8
~~~js
db.students.find({$and:[{"birth_year":{$gte:1990}},{"birth_year":{$lte:2000}},{"gender":{$eq:"M"}}]})
~~~
## Ex9
~~~js
db.students.find({"birth_year":{$not:{$eq:1985}}})
~~~
## Ex10
~~~js
db.students.find({$or:[{"birth_year":{$eq:1970}},{"birth_year":{$eq:1980}},{"birth_year":{$eq:1990}}]})
~~~
## Ex11
~~~js
db.students.find({$and:[{"birth_year":{$not:{$eq:1970}}},{"birth_year":{$not:{$eq:1980}}},{"birth_year":{$not:{$eq:1990}}}]})
~~~
## Ex12
~~~js
db.students.find({"birth_year": {$mod:[2.0,0]}})
~~~
## Ex13
~~~js
db.students.find({"phone_aux": {$exists:true}})
~~~
## Ex14
~~~js
db.students.find({"phone_aux": {$exists:false}})
~~~
## Ex15
~~~js
db.students.find({"lastname2": {$exists:false}})
~~~
## Ex16
~~~js
db.students.find({"phone_aux": {$exists:true},"lastname2": {$exists:false}})
~~~
## Ex17
~~~js
db.students.find({"email": /\.net$/i})
~~~
## Ex18
~~~js
db.students.find({"phone": /^622/})
~~~
## Ex19
~~~js
db.students.find({"dni": /^[A-Z].+[A-Z]$/})
~~~
## Ex20
~~~js
db.students.find({"name": /^(a|e|i|o|u)/i})
~~~
## Ex21
~~~js
db.students.find({"name": /.+ .+/i})
~~~
## Ex22
~~~js
db.students.find({"name": /.............+/i})
~~~
o
~~~js
db.students.find({ "name": { $regex: /.{13,}/ } })
~~~
## Ex23
~~~js
db.students.find({name:/.*[aeiouàáèéíòóú].*{3}/i})
~~~
## Ex24
~~~js
db.bios.find({"contribs": {$in:["OOP"]}})
~~~
## Ex25
~~~js
db.bios.find({"contribs": {$in:["OOP","Java"]}})
~~~
## Ex26
~~~js
db.bios.find({"contribs": {$in:["OOP"]},"contribs": {$in:["Simula"]}})
~~~
## Ex27
~~~js
db.bios.find({"deathYear": {"$exists":false})
~~~
## Ex28
~~~js
db.bios.find({"deathYear": {"$exists":true})
~~~
## Ex29
~~~js
db.bios.find({"award.year": {$eq:[2002]}})
~~~
## Ex30
~~~js
db.bios.find({"awards": {$size:3}})
~~~
## Ex31
~~~js
db.students.find({name:/.*([aeiouàáèéíòóú].*){3}.*/i})
~~~
## Ex32
~~~js
~~~
## Ex33
~~~js
~~~
## Ex34
~~~js
~~~
## Ex35
~~~js
~~~
## Ex36
~~~js
~~~
## Ex37
~~~js
~~~
## Ex38
~~~js
~~~
## Ex39
~~~js
~~~
