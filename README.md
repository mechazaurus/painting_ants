# Projet Java Performance - Painting Ants

## Initialisation de l'application (branche init)

### Instanciation de la couleur de fond

Lors du lancement de l'application, la toile de fond est instanciée avec une couleur blanche : **_new Color(255,255,255)_**

Le problème est que pour chaque pixel de l'image, une nouvelle couleur est créée.
Ainsi, il y a autant de couleurs créées que de pixels dans l'image.
Cela pose des problèmes d'occupation de la mémoire et de vitesse d'exécution.
En effet, chaque instanciation de nouvel objet prend du temps et même s'il est négligeable pour un objet, il devient conséquent pour un grand nombre d'objets.

#### JMH

**Paramètrages**

|Mode|Throughput|
|:---|:---|
|Warmups|5 itérations, 5 secondes|
|Measurments|5 itérations, 5 secondes|

**Résultats**

|Benchmark|Mode|Cnt|Score|Error|Units|
|:---|:---|:---|:---|:---|:---|
|MyBenchmark.benchmarkFillColorWithNew|thrpt|25|21,553|± 0,404|ops/s|
|MyBenchmark.benchmarkFillColorWithStatic|thrpt|25|148,164|± 31,369|ops/s|

#### Méthode de récupération de la couleur

Les fourmis récupèrent la valeur de la couleur du pixel sur lequel elles sont grâce à une méthode **_getColor()_** qui est synchronisée. Le fait de synchroniser cette méthode donne l'accès à une seule fourmi à la fois. Cependant, son but est uniquement de retourner une valeur et elle n'est pas sensée être concurrente puisque c'est une lecture.
