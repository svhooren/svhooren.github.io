# Svhooren
## Mémoire - Mars

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

