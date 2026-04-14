# Notes du projet

travail sur données spatiales : latitude/longitude/temporel (mesures saisonnieres/année-mois-jour...)

choix de focus le projet sur la pollution 

## Indicateur pollution : PM 

### Définition et principales sources des particules fines

Les particules en suspension (notées « PM » en anglais pour « Particulate matter ») sont d’une manière générale les fines particules solides portées par l’eau ou solides et/ou liquides portées par l’air (Wikipédia).

Pour faire simple, les particules fines, c’est de la poussière.
Les particules ou poussières proviennent de sources naturelles (sel de mer, feux de forêt et érosion des sols par le vent) comme d’activités humaines (transport, chauffage, industries). Selon leur taille, elles pénètrent plus ou moins dans l’appareil respiratoire et sont donc plus ou moins dangereuses. Leur dangerosité dépend également de leur composition. Elles sont nocives pour la santé respiratoire et cardiovasculaires.

PM10 ou PM2,5 --> explications :
Les particules en suspension PM10 sont des particules dont le diamètre est inférieur à 10 micromètres (poussières inhalables),
Les particules en suspension PM2.5 sont inférieur à 2.5 micromètres et pénètrent plus profondément dans l’appareil respiratoire.

 
## Notions à intégrer dans le TER
* Isotropie : Un phénomène est isotrope si son comportement est le même dans toutes les directions.
 * ➡️ la distance entre deux points suffit à expliquer leur relation
 * ➡️ la direction n’a aucune importance
<br>

* Anisotropie : Un phénomène est anisotrope si son comportement change selon la direction.
 * ➡️ distance + direction sont importantes
 * ➡️ le phénomène n’est pas homogène dans l’espace
<br>

* Variogramme : "comment la similarité entre deux points diminue avec la distance"
  * Imagine que tu mesures la pollution de l’air en plusieurs points d’une ville :
    * Deux points très proches → valeurs similaires
    * Deux points éloignés → valeurs plus différentes
* 👉 Le variogramme quantifie cette idée.
* Différents paramètres du variogramme :
 * nuggets (effet pépité) : bruit/erreur de mesure
 * range (portée) : distance de corrélation
 * sill (palier) : variance totale
<br>

* Stationnarité : “Les propriétés statistiques ne changent pas dans l’espace”
  * Si la structure est stationnaire : le comportement est le même partout seule la distance compte
  * Si non stationnaire : il y a des zones différentes (ville vs campagne)
 * Deux types de stationnarité :
  * Faible : moyenne constante, covariance dépend seulement de la distance => phénomène homogène : la pollution a le même comportement statistique, seules les distances comptent
  * Intrinsèque : variance dépend seulement de h (distance entre 2 points dans l'espace) => la façon dont ça varie est stable, moyenne pas constante et pollution pas la meme partout
   * 💡 Exemple très parlant
   * Cas pollution : Centre-ville : 40 µg/m³, Banlieue : 25 µg/m³
     * ➡️ les niveaux sont différents ❌ (pas stationnaire faible), mais : entre deux points à 1 km → différence faible, entre deux points à 20 km → différence plus grande 👉 ce schéma reste le même partout
     * ➡️ ça = stationnarité intrinsèque ✅ 
<br>

* Interpolation : méthode d'estimation pour estimer la valeur d'un point inconnu en fonction des points observés
  * Simple : “Je devine avec une règle simple (souvent géométrique)”
  * Kriging (interpolation optimale) : “Je prédis avec un modèle statistique basé sur la corrélation spatiale” => utilise la covariance donc kernel, kriging utilise soit la covariance soit un variogramme
<br>

* Noyaux de covariance Kernel (Exponential, Gaussian, Matérn) : décrire comment deux points sont corrélé 

## source des datas : 
https://www.geodair.fr/donnees/consultation

## bib
https://desktop.arcgis.com/fr/arcmap/latest/extensions/geostatistical-analyst/choosing-a-lag-size.htm
https://www.miningdoc.tech/wp-content/uploads/2024/10/05-Geostatistics-Variograms.pdf
https://vsp.pnnl.gov/help/vsample/Kriging_Variogram.htm
