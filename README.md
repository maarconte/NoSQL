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
$ use million
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
- Les meilleures entrées des films américains
```sh
db.getCollection('films').find({nationalité:"ETATS UNIS"}).sort({"entrées (millions)": -1})
```
- Trouver les films qui ont fait plus de 5 millions d'entrées
- Trier les films par date
- Trier par nationalités
- Nombre d'entrées par année
- Top 3 des films qui ont fait le plus d'entrées
- Top 3 des films par pays 
- Top 3 des nationalités des films qui ont le plus d'entrées
- Le nombre d'entrées par mois et par an
- Le nombre d'entrées en moyenne par mois
- Le nombre de films sortis par mois
- La moyenne de nombre de films sortis par mois