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
Import CSV file
```sh
$ tail -n +7 [file] | sed 's/;;;;//g' | tr ";" "\t" | sed "$d" | mongoimport --db million --collection films --type tsv --headerline
```

## Requêtes
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