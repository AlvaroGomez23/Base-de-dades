### Excercicis MongoDB



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

```

6. Dona el número total de punts de l'equip local del partit amb codi_acb :"103778"
```JavaScript

``` 
7. Obtenir els punts de cada equip (local i visitant) del partit amb codi_acb: "103778"
```JavaScript

```
