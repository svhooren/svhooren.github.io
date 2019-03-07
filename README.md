# Svhooren
## Mémoire - Mars

Après avoir exploré la littérature pour les deux parties précédentes (Etat de l'art et Méthodologie), je commence le traitement de données.

J'ai trouvé mes données sociologiques en ligne sur le site de l'![INSEE](https://www.insee.fr/fr/statistiques/2028584) et mes données électorales sur le site [Open Data](https://public.opendatasoft.com/explore/dataset/election-presidentielle-2017-resultats-par-bureaux-de-vote-tour-1/table/?flg=fr&disjunctive.libelle_de_la_commune&refine.libelle_de_la_commune=Paris).


Pour le moment je bloque sur une ventilation de données. Mes données sociologiques sont à l'échelle IRIS (Ilots regroupés pour l'information statistique) et mes données électorales sont à l'échelle du bureau de vote. Afin de les mettre à la même échelle du bureau de vote j'essaye de réaliser une ventilation grâce au script [spReaportion](https://github.com/joelgombin/spReapportion) de Joël Gombin. J'ai déjà rencontré plusieurs problèmes, notamment des données excell avec une colonne contenant des informations géographiques et qu'il a donc fallu faire passer en objet orienté R. Pour cela j'ai eu de l'aide de pokyah qui m'a réalisé un script. 

```R
load("~/Downloads/ParisPollingStations2012.rda")
load("~/Downloads/ParisIris.rda")

# convert to sf
ParisIris = sf::st_as_sf(ParisIris)
ParisPollingStations2012 = sf::st_as_sf(ParisPollingStations2012)

# plot the maps
mapview::mapview(ParisIris)
mapview::mapview(ParisPollingStations2012)

# read excel
bureaux <- read_delim("~/Documents/MA2/Mémoire/Memoire_V1/data_raw/secteurs-des-bureaux-de-vote.csv", 
                                           +     ";", escape_double = FALSE, trim_ws = TRUE)

# splitting the geo_point_2d in to columns lon and lat
bureaux = bureaux %>%
  + tidyr::separate(geo_point_2d, c("lat", "lon"), ",") %>%
  + dplyr::mutate_at(vars(c("lon","lat")), funs(as.numeric)) %>%
  + dplyr::select(-one_of("geo_shape"))

# making it a sf 
bureaux_sf = sf::st_as_sf(bureaux_sf, coords = c("lon", "lat"))

# view it on a map
mapview::mapview(bureaux_sf)

# convert to sp

bureaux_sp = as(bureaux_sf, "Spatial")
```
Malgré cette conversion de données, je reste bloqué avec une match error : 
![alt text](https://github.com/svhooren/svhooren.github.io/blob/master/Error_march.png)

J'ai posé une issue sur Github à Joël Gombin et contacté une personne qui a travaille sur ce script et l'a actualisé pour voir si ils n'avaient pas rencontré ce même problème ou si ils n'avaient pas de meilleures données à me proposer.


## Mémoire
Pour le moment j'ai déjà réalisé mon état de l'art ainsi que ma méthodologie. Dans le but de me forcer à un apprentissage de différents languages, j'ai décidé de rédiger mon mémoire en LaTeX, mais également de tenir un blog via Github Pages. Je souhaite également mettre la version finale sur une Shiny App qui sera accessible depuis cette page. Cette Shiny App et la page sont rédigées respectivement R et en Markdown. 
