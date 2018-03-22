# 1 million
## Launch Mongodb
```sh
sudo mongod
```
```sh
mongo
```
## Install db
Create database
```sh
$ use millions
```

Import Json
```sh
mongoimport --db millions --collection films ./db_millions.json --jsonArray
```

## Requêtes
- Transformer les entrées de type string en float
```sh
db.films.find({entrees: {$exists: true}}).forEach(function(obj) { 
    obj.entrees = parseFloat(obj.entrees);
    db.films.save(obj);
});
```
- Transformer les noms de pays en minuscule en majuscule (France -> FRANCE)
```sh
db.films.updateMany({"nationalite": "France"},{$set: {"nationalite": "FRANCE"}})
```
- Afficher la liste des nationalites
```sh
db.films.aggregate( [ { $group : { _id : "$nationalite"} } ] )
```
- Nombre de films par nationalites
```sh
db.films.aggregate( [ { $group : { _id : "$nationalite", count:{$sum:1} } } ] )
```
- Afficher tous les Harry Potter
```sh
db.films.find({titre:/^HARRY POTTER/})
```
- Ajouter le genre "Fantastique" aux films Harry Potter
```sh
db.films.updateMany({titre: /^HARRY POTTER/},{ $set: { genre: "Fantastique" }})
```
- Ajouter le champ annee à tous les films
```sh
db.films.updateMany({'sortie': /08$/}, {'$set': {'annee': NumberInt(2008)}})
```
- Transformer un float en int
```sh
db.films.updateMany({'sortie': /08$/}, {'$set': {'annee': NumberInt(0)}})
```
- Les meilleures entrées des films américains
```sh
db.films.find({nationalite:"ETATS UNIS"}).sort({"entrees": -1})
```
- Trouver les films qui ont fait plus de 5 millions d'entrées
```sh
db.films.find({entrees: {$gt: 4.99}}).sort({entrees: -1})
```
- Films de 2008
```sh
db.films.find({sortie: /08$/})
```
- Trier les films par nationalites
```sh
db.films.find({},{nationalite: 1, titre: 1}).sort({nationalite: 1})
```
- Nombre d'entrées par annee
```sh
db.films.aggregate([{$group: { _id: "$annee", countFilms: { $sum: 1}, countEntrees: {$sum: "$entrees"}}},{$sort:{countEntrees: -1}}]);
```
- Top 3 des nationalites les plus représentées
```sh
db.films.aggregate([{$group: { _id: "$nationalite", count: { $sum: 1}}},{$sort:{count: -1}}]);
``` 
- Top 3 des films qui ont fait le plus d'entrées
```sh
db.films.find({}).sort({entrees: -1}).limit(3)
```
- Top 3 des nationalites des films qui ont le plus d'entrées
```sh
db.films.aggregate([{$group: { _id: "$nationalite", count: { $sum: 1}, countEntrees: {$sum: "$entrees"}}},{$sort:{count: -1}},{$limit: 3}]);
```