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
db.films.updateMany({"nationalité": "France"},{$set: {"nationalité": "FRANCE"}})
```
- Afficher la liste des nationalités
```sh
db.films.aggregate( [ { $group : { _id : "$nationalité"} } ] )
```
- Nombre de films par nationalités
```sh
db.films.aggregate( [ { $group : { _id : "$nationalité", count:{$sum:1} } } ] )
```
- Afficher tous les Harry Potter
```sh
db.films.find({titre:/^HARRY POTTER/})
```
- Ajouter le genre "Fantastique" aux films Harry Potter
```sh
db.films.updateMany({titre: /^HARRY POTTER/},{ $set: { genre: "Fantastique" }})
```
- Ajouter le champ année à tous les films
- Les meilleures entrées des films américains
```sh
db.films.find({nationalité:"ETATS UNIS"}).sort({"entrees": -1})
```
- Trouver les films qui ont fait plus de 5 millions d'entrées
```sh
db.films.find({entrees: {$gt: 4.99}}).sort({entrees: -1})
```
- Trier les films par date
```sh
db.films.find({sortie: /08$/})
```
- Trier les films par nationalités
```sh
db.films.find({},{nationalité: 1, titre: 1}).sort({nationalité: 1})
```
- Nombre d'entrées par année
- Top 3 des nationalités les plus représentées
```sh
db.films.aggregate([{$group: { _id: "$nationalité", count: { $sum: 1}}},{$sort:{count: -1}}]);
``` 
- Top 3 des films qui ont fait le plus d'entrées
```sh
db.films.find({}).sort({entrees: -1}).limit(3)
```
- Top 3 des films par pays s
- Top 3 des nationalités des films qui ont le plus d'entrées
```sh
db.films.aggregate([{$group: { _id: "$nationalité", count: { $sum: 1}, countEntrees: {$sum: "$entrees"}}},{$sort:{count: -1}},{$limit: 3}]);
```
- Le nombre d'entrées par mois et par an
- Le nombre d'entrées en moyenne par mois
- Le nombre de films sortis par mois
- La moyenne de nombre de films sortis par mois