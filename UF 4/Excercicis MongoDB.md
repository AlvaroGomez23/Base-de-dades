### Excercicis MongoDB
## Excercicis Students

1. Busca els estudiants de gènere masculí
```javascript
db.students.find({gender: "H"})
```
2. Digues quina és la quantitat d’estudiants de gènere femení.
```javascript
db.students.count({gender: "M"})
```
3. Busca els estudiants nascuts l’any 1993
```javascript
db.students.find({birth_year: 1993})
```
4. Busca els estudiants de gènere masculí i nascuts l’any 1993
```javascript
db.students.find({birth_year: 1993},{gender: "M"})
```
5. Busca els estudiants nascuts després de l’any 1990
```javascript
db.students.find({birth_year: {$gt: 1990}})
```
6. Busca els estudiants nascuts abans o durant l’any 1990
```javascript
db.students.find({birth_year: {$gte: 1990}})
```
7. Busca els estudiants nascuts a la dècada dels 90s
```javascript
db.students.find({$and: [{birth_year: {$gte: 1990}}, {birth_year: { $lte: 1999}}]})
```
8. Busca els estudiants de gènere femani nascus a la dècada dels 90s
```javascript
db.students.find({$and: [{birth_year: {$gte: 1990}}, {birth_year: { $lte: 1999}},{gender: "M"}]})
```
9. Busca els estudiants que no han nascut a l’any 1985
```javascript
db.students.find({birth_year: {$ne: 1985 } } )
```
10. Busca aquells estudiants que han nascut l’any 1970,1980 o 1990
```javascript
db.students.find({birth_year: { $in:[1970,1980,1990] }})
```
11. Busca aquells estudiants que no han nascut l’any 1970,1980 o 1990
```javascript
db.students.find({birth_year: { $in:[1970,1980,1990] }})
```
12. Busca aquells estudiants nascuts en any parell.
```javascript
db.students.find({ "birth_year": { $mod: [2, 0] } })
```
13. Busca els estudiants que tinguin telèfon auxiliar
```javascript
db.students.find({ "phone_aux": { "$exists": true } })
```
14. Busca els estudiants que no tinguin telèfon auxiliar
```javascript
db.students.find({ "phone_aux": { "$exists": false } })
```
15. Busca els estudiants que no tinguin segon cognom
```javascript
db.students.find({ "lastname2": { "$exists": false } })
```
16. Busca els estudiants que tinguin telèfon auxiliar i només el primer cognom
```javascript
db.students.find({ "phone_aux": { "$exists": true },
 "lastname1": { "$exists": true },                  
 "lastname2": { "$exists": false }
 })
```
17. Busca els estudiants que el seu correu electrònic acabi en .net
```javascript
db.students.find({email:/.net$/})
```
18. Busca els estudiants que el seu telèfon comenci per 622
```javascript
db.students.find({phone:/^622/})
```
19. Busca els estudiants que el seu dni comenci i acabi amb una lletra
```javascript
db.students.find({dni:/^[a-z].+[a-z]$/i}) <<- i = Ignore case
```
20. Busca els estudiants que el seu nom comenci per una vocal
```javascript
db.students.find({name:/^[aeiou]/i})
```
21. Busca els estudiants que el seu nom sigui compost
```javascript
db.students.find({name:/\w\s\w/i})
```
22. Busca els estudiants amb un nom més llarg de 13 caràcters
```javascript

```
23. Busca els estudiants amb 3 o més vocals en el seu nom
```javascript

```

## Excercicis DB_ACB
1. Volem comparar l'alçada dels dos germans Gasol. El noms curts són "Pau Gasol" i
"Marc Gasol".
```javascript
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        nom_curt: {
          $in: ["Pau Gasol", "Marc Gasol"]
        }
      }
  }
]
```
2. L’entrenador “Pedro Martínez” és un dels entrenadors més veterans. Quants partits
ha participat com a entrenador. Independentment de si ho ha fet com a local o com
a visitant. Restringeix la consulta als partits de lla Lliga Regular de la temporada
2023-2024.
```JavaScript
db.partits.countDocuments({
   $or: [
      { "equip_local.entrenadors.nom": "Pedro Martínez" },
      { "equip_visitant.entrenadors.nom": "Pedro Martínez" }
   ],
   "temporada": "2023-2024",
   "competicio": "Lliga Regular"
})
```
3. La llicència "JFL" indica que és un jugador de formació. Quants jugadors tenim amb
aquesta llicència?.
```JavaScript
db.getCollection('jugadors').aggregate(
  [
    { $match: { llicencia: { $eq: 'JFL' } } },
    { $count: 'llicencia' }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```
4. Passa a Decimal el camp alcada de la col·lecció jugadors.
(https://www.mongodb.com/docs/manual/reference/operator/aggregation/convert/)
```JavaScript

```
5. Quins jugadors tenim el compte d'Instagram? Mostra el nom_curt del jugador i
l'usuari d'Instragram.
```JavaScript
db.jugadors.find({xarxes_socials: {$elemMatch: {"nom" : "instagram"}}},{"xarxes_socials.$":1})
```

6. Dona el número total de punts de l'equip local del partit amb codi_acb :"103778"
```JavaScript
db.partits.aggregate([{ $match: { codi_acb: "103778" } },{ $addFields: { "equip_local.punts": { $toInt: "$equip_local.punts" } } },{ $project: { _id: 1, "equip_local.punts": 1 } }])
``` 
7. Obtenir els punts de cada equip (local i visitant) del partit amb codi_acb: "103778"
```JavaScript

```
