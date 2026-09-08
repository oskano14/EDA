---
updated: 2026-09-04T12:32:39.459+02:00
edited_seconds: 40
---
#### Contexte

Le data engineer de Lumina & Co vous a livré deux fichiers extraits du CRM : `customers.csv` et `transactions.csv`. Avant de pouvoir segmenter quoi que ce soit, vous devez comprendre ce que vous avez, et corriger les problèmes.

_"Je vous préviens,"_ dit-il, _"l'export n'est pas parfait. Ça vient de notre CRM, et quelques listes que des employés tenaient à jour de leur côté"_

#### Données

- `customers.csv` : un profil par client (customer_id, country, dates, métriques agrégées)
- `transactions.csv` : historique ligne par ligne (invoice_id, customer_id, product_code, quantity, unit_price, invoice_date, country)

#### Étape 1 : Chargement

Chargez les deux fichiers et répondez aux questions suivantes avant d'écrire la moindre ligne de nettoyage :

- Combien de clients ? Combien de transactions ? Sur quelle période ?
- Quelles colonnes contiennent des valeurs manquantes, et dans quelle proportion ?
- Quels types de données sont attendus vs observés ?
- Y a-t-il des doublons ?

> avant de "nettoyer", documentez ce que vous voyez. Un biais non documenté est un biais invisible. Construisez un data quality report minimaliste (5 observations clés) avant de toucher aux données.

#### Étape 2 : Détection et traitement des anomalies

Inspectez et traitez les problèmes suivants. Pour chacun, documentez votre décision (pourquoi ce traitement et pas un autre) :

**Dans `transactions.csv` :**
- Les `customer_id` manquants : que représentent ces lignes ? Faut-il les supprimer ou les conserver ?
- Les `quantity` négatives : d'où viennent-elles ? Quel impact sur les agrégations futures ?
- Les `unit_price` à zéro : anomalie ou cas métier légitime ?
- Les `product_code` atypiques : identifiez les codes non-produits (frais de port, ajustements comptables, etc.)
- Calculez `line_total` = quantity × unit_price. Vérifiez la cohérence.

**Dans `customers.csv` :**
- Cohérence des dates (`first_purchase` ≤ `last_purchase`)
- Valeurs aberrantes dans `total_spent`, `n_orders`, `avg_basket`
- Clients avec une seule transaction vs clients récurrents : quelle proportion ?

#### Étape 3 : Analyse exploratoire orientée marketing
Une fois les données nettoyées, conduisez une EDA ciblée pour répondre à des questions business précises.

**Distribution des achats :**
- Distribution du montant des paniers (attention aux outliers : clients B2B ?)
- Distribution de la fréquence d'achat par client
- Quelle proportion de clients génère 80% du chiffre d'affaires ? (loi de Pareto)

**Saisonnalité :**
- Évolution du chiffre d'affaires mensuel sur la période disponible
- Y a-t-il des pics saisonniers visibles ? Lesquels ? Qu'impliquent-ils pour les campagnes marketing ?

**Géographie :**

- Répartition des clients par pays
- Les comportements d'achat (panier moyen, fréquence) varient-ils selon la géographie ?
- Y a-t-il un biais géographique dans la base ? (surreprésentation d'un pays ?)

> **Point de vigilance :** si un pays est surreprésenté dans la base initiale, les segments que vous allez créer demain refléteront ce biais. Documentez-le, ce sera une limite à mentionner dans votre présentation finale.

**Relations entre variables :**

- Corrélation entre récence et montant dépensé : est-elle linéaire ?
- Relation entre fréquence et panier moyen : les clients fréquents dépensent-ils plus par visite ?
- Y a-t-il des segments "naturellement visibles" dans les données avant toute modélisation ?

**Catégories de produits :**

- Quelle est la répartiation du chiffre d'affaires par `category` ?
- Une catégorie domine-t-elle, ou la répartition est-elle équilibrée ?
- Ce premier regard par catégorie vous servira de point de comparaison en Jour 2, quand vous chercherez à savoir si certains segments RFM ont une affinité produit particulière.

**Données zero-party (`customers.csv`) :**

- Pour chacune des colonnes `declared_preference`, `age_bracket`, `life_stage`, `urban_density` : quelle proportion de clients a une valeur renseignée ?
- Ces taux de remplissage sont-ils cohérents avec ce qu'on attend d'une donnée zero-party ?


#### Étape 4 : Formulation des hypothèses marketing

Sur la base de votre EDA, formulez 3 à 5 hypothèses que vous chercherez à valider ou invalider avec la segmentation du Jour 2. Format suggéré :

> _"Je suppose que [observation EDA] implique [comportement client attendu], ce qui suggère [action marketing potentielle]."_

Ces hypothèses structureront votre narration dans la présentation finale.

---
## Livrable de fin de journée à publier sur GitHub

**Notebook de nettoyage et EDA** : data quality report documenté, décisions de traitement justifiées, 3 à 5 hypothèses marketing formulées.