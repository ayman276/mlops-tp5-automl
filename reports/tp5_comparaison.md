# Modele manuel ou modele AutoML ? (TP05)

| Critere                    | Manuel (rf 500/16)                              | AutoML (FLAML 90 s)                                   |
|----------------------------|-------------------------------------------------|-------------------------------------------------------|
| Famille de modele          | foret aleatoire                                 | xgboost (322 arbres, max_leaves = 4)                  |
| F1 sur le jeu de test      | 0.8669                                          | 0.8561                                                |
| Temps de recherche machine | 1.6 s (un seul entrainement)                    | 90.1 s (exploration de plusieurs configurations)      |
| Temps humain (estimation)  | ~1 h                                            | ~10 min                                               |
| Latence mediane (ms)       | 31.5 / 45.7 (2 mesures)                         | 11.2 / 11.3 (2 mesures)                               |
| Latence p95 (ms)           | 49.4 / 52.1 (2 mesures)                         | 26.5 / 22.6 (2 mesures)                               |
| Taille du fichier (Mo)     | 13.37                                           | 0.28                                                  |
| Explicabilite              | importance des variables, 500 arbres,           | importance des variables, 322 arbres peu profonds,    |
|                            | profondeur 16 (hyperparametres lisibles)        | mais hyperparametres cfg_ moins intuitifs             |
|                            |                                                 | (learning_rate, reg_alpha, reg_lambda...)             |

## Decision (5 lignes) : quel modele mettre en production, et pourquoi ?

Je mettrais le modele AutoML (xgboost) en production. Son F1 (0.8561) n'est
inferieur que de 0.011 a celui du modele manuel (0.8669). Il est environ 48 fois
plus leger (0.28 Mo contre 13.37 Mo), ce qui compte pour l'image Docker, et sa
latence mediane est 3 a 4 fois plus faible (~11 ms contre 31 a 46 ms), avec un
meilleur p95. Le modele manuel reste preferable si l'explicabilite ou le F1 maximal priment.