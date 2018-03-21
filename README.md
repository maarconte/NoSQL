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

var cursor = db.films.find(); 
while (cursor.hasNext()) { 
  var doc = cursor.next(); 
  db.films.update({_id : doc._id}, {$set : {"entrée (millions)" : new NumberInt('doc.entrées (millions)') }});
}

db.films.updateMany({},{ $rename: { 'entrées (millions)': 'entrees'} } )