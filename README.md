# EDA marketing - Lumina & Co

Projet fil rouge de data marketing consacré au nettoyage, à l'analyse
exploratoire et à la segmentation des données CRM de Lumina & Co.

## Livrable Jour 1

Le notebook [01_eda_nettoyage_lumina.ipynb](01_eda_nettoyage_lumina.ipynb)
contient :

- le rapport de qualité initial des tables clients et transactions ;
- les décisions de nettoyage documentées ;
- l'analyse des paniers, de la fréquence et de la concentration du chiffre
	d'affaires ;
- les analyses temporelles, géographiques et produit ;
- cinq hypothèses marketing à tester pendant la segmentation RFM.

## Installation

Le projet utilise Python 3 et Jupyter.

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

## Données

Placer les fichiers `customers.csv` et `transactions.csv` dans le dossier
`Lumina & Co - CRM/`, puis exécuter le notebook de haut en bas.

Les CSV bruts ne sont pas versionnés : `transactions.csv` dépasse notamment
la limite de taille standard de GitHub et les données CRM doivent rester
locales. Le notebook ne modifie jamais les fichiers sources.

## Premiers résultats

- 50 295 clients et 1 613 433 transactions entre janvier 2022 et juin 2026 ;
- 22,77 % des transactions sans identifiant client ;
- 28,8 % de clients avec une seule commande ;
- 34,3 % des clients génèrent 80 % du chiffre d'affaires client ;
- 91,3 % des clients sont en France ;
- couverture des variables zero-party comprise entre 19,8 % et 42,7 %.
