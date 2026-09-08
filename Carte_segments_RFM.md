# Carte des segments RFM - Lumina & Co

## Méthode

- Population : 49 184 clients disposant d'au moins une vente positive.
- Snapshot date : 30 juin 2026, date maximale de `transactions.csv`.
- Récence : jours depuis la dernière facture de vente positive.
- Fréquence : nombre de factures de vente positives distinctes.
- Montant : somme nette des lignes commerciales, retours compris.
- Scores : quintiles de 1 à 5; les ex aequo de fréquence restent regroupés.

## Vue d'ensemble

| Priorité | Segment | Clients | Base | CA net | R médiane | F médiane | M médian |
| ---: | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| 1 | À risque | 4 704 | 9,6 % | 8,1 % | 454 j | 4 | 355 € |
| 2 | Champions | 9 334 | 19,0 % | 61,7 % | 53 j | 9 | 826 € |
| 3 | Potentiels fidèles | 9 420 | 19,2 % | 12,5 % | 126 j | 3 | 247 € |
| 4 | Nouveaux prometteurs | 4 068 | 8,3 % | 0,8 % | 62 j | 1 | 71 € |
| 5 | Fidèles | 4 338 | 8,8 % | 8,8 % | 181 j | 7 | 498 € |
| 6 | Occasionnels | 6 574 | 13,4 % | 6,1 % | 409 j | 1 | 199 € |
| 7 | Perdus | 10 746 | 21,8 % | 2,0 % | 647 j | 1 | 69 € |

## Fiches d'activation

### 1. À risque

Clients historiquement actifs et valorisés, sans achat récent. Leur valeur est
menacée, ce qui justifie une intervention prioritaire.

**Action :** relance personnalisée en deux temps avec rappel des produits déjà
achetés, puis avantage limité si le premier message ne suffit pas.

**Mesure :** taux de réactivation à 30 jours.

### 2. Champions

Clients récents, très fréquents et à forte valeur. Ils concentrent 61,7 % du CA
net et doivent être protégés sans les habituer aux remises.

**Action :** programme VIP, accès anticipé et recommandations personnalisées.

**Mesure :** taux de réachat et valeur à 90 jours.

### 3. Potentiels fidèles

Clients assez récents ayant déjà commandé plusieurs fois. Ils peuvent évoluer
vers une relation régulière.

**Action :** séquence de nurturing et vente croisée fondée sur la dernière
catégorie achetée.

**Mesure :** passage à trois commandes sous 90 jours.

### 4. Nouveaux prometteurs

Clients récents avec une seule commande et une valeur encore limitée. L'enjeu
est de déclencher rapidement le deuxième achat.

**Action :** onboarding post-achat, conseils d'utilisation et rappel adapté au
cycle de consommation du produit.

**Mesure :** taux de deuxième commande à 30 jours.

### 5. Fidèles

Clients fréquents et valorisés, mais moins récents ou moins dominants que les
Champions.

**Action :** programme de fidélité et rappels de réapprovisionnement automatisés.

**Mesure :** fréquence et panier sur 90 jours.

### 6. Occasionnels

Clients au comportement irrégulier et au potentiel incertain. Le coût de contact
doit rester faible.

**Action :** campagnes email automatisées et tests de contenu sans média payant.

**Mesure :** revenu incrémental par email envoyé.

### 7. Perdus

Clients anciens, peu fréquents et à faible valeur. Ils occupent 21,8 % de la
base, mais ne produisent que 2,0 % du CA net.

**Action :** exclusion des médias payants et campagne de permission occasionnelle.

**Mesure :** coût évité et taux de réactivation organique.

## Enrichissement zero-party

La préférence déclarée est renseignée pour 38,3 % du segment À risque. Parmi
ces répondants, `Anti-imperfections` arrive en tête avec 15,5 %, contre 14,4 %
dans l'ensemble des répondants. L'écart est faible : il suggère un angle de
message à tester, mais ne justifie pas à lui seul une nouvelle sous-segmentation.

Le canal ne peut pas être personnalisé avec les seules tables clients et
transactions. Tout envoi email reste soumis à la vérification de la base légale
et du droit d'opposition.

## Limite de qualité majeure

Les agrégats reconstruits depuis `transactions.csv` concordent imparfaitement
avec les indicateurs pré-calculés de `customers.csv` : 11,93 % pour la récence,
78,18 % pour la fréquence et 6,90 % pour le montant à deux centimes près. Les
segments respectent donc la consigne du TP et utilisent exclusivement les
transactions disponibles. Une activation réelle nécessiterait une réconciliation
des deux sources avec l'équipe responsable des données.
