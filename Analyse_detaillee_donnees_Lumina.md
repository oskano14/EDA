# Analyse détaillée des données Lumina & Co

## 1. Comprendre le projet

Lumina & Co dispose d'un historique de ventes de produits cosmétiques et d'un fichier CRM contenant un profil agrégé par client. Le projet cherche à répondre à trois questions :

1. **Que contient réellement la donnée et peut-on lui faire confiance ?**
2. **Quels comportements d'achat structurent la clientèle ?**
3. **Quels groupes doivent recevoir une action marketing différente ?**

Le travail est donc organisé en trois niveaux :

- **Jour 1 :** audit, nettoyage et analyse exploratoire ;
- **Jour 2 :** calcul des indicateurs RFM et création de sept segments ;
- **Activation :** adaptation de l'objectif au segment, de l'offre à la préférence déclarée et du ton à la tranche d'âge.

L'analyse est arrêtée au **30 juin 2026**, dernière date disponible dans les transactions.

## 2. Les deux sources de données

### Fichier clients

`customers.csv` contient **50 295 profils** et **13 variables**.

| Groupe | Variables | Interprétation |
|---|---|---|
| Identité technique | `customer_id` | Clé de rattachement, pas une mesure métier |
| Géographie | `country` | Pays du profil client |
| Historique CRM | `first_purchase`, `last_purchase`, `tenure_days` | Ancienneté et bornes d'activité enregistrées dans le CRM |
| Agrégats CRM | `n_orders`, `total_spent`, `avg_basket`, `recency_days` | Résumé pré-calculé de la relation commerciale |
| Données déclaratives | `declared_preference`, `age_bracket`, `life_stage`, `urban_density` | Informations fournies par le client, donc non déduites des achats |

Chaque identifiant est unique, aucun doublon client n'est présent et aucune date de premier achat n'est postérieure à la date de dernier achat.

### Fichier transactions

`transactions.csv` contient initialement **1 613 433 lignes** et **9 variables**.

| Groupe | Variables | Interprétation |
|---|---|---|
| Facture | `invoice_id`, `invoice_date` | Commande et date associée |
| Client | `customer_id`, `country` | Rattachement au client et pays de la transaction |
| Produit | `product_code`, `product_name`, `category` | Référence et famille produit |
| Valeur | `quantity`, `unit_price` | Quantité et prix unitaire |

Une ligne représente une **ligne de facture**, pas nécessairement une commande complète. Une commande peut donc apparaître sur plusieurs lignes.

## 3. De la donnée brute à la population analysée

| Étape | Volume | Décision |
|---|---:|---|
| Profils CRM | 50 295 clients | Base descriptive initiale |
| Lignes transactionnelles brutes | 1 613 433 | Historique avant nettoyage |
| Doublons exacts | 445 lignes | Suppression pour éviter le double comptage |
| Lignes après dédoublonnage | 1 612 988 | Base transactionnelle nettoyée |
| Lignes commerciales identifiées | 1 240 685 | Client connu, prix non négatif, frais et écritures techniques exclus |
| Ventes produit positives identifiées | 1 211 343 | Base des fréquences, récences et paniers positifs |
| Clients ayant au moins une vente positive | 49 184 | Population utilisée pour la segmentation RFM |
| Profils CRM sans vente positive exploitable | 1 111 | Exclus du RFM, mais conservés dans la source CRM |

La différence entre **50 295 profils CRM** et **49 184 clients segmentables** n'est donc pas une erreur : le RFM exige au moins un achat positif identifiable.

## 4. Qualité et anomalies

### Identifiants manquants

**367 328 lignes**, soit **22,77 %** des transactions brutes, n'ont pas de `customer_id`. Elles peuvent contribuer à l'analyse globale du chiffre d'affaires, mais pas à une analyse de parcours, de fidélité ou de valeur individuelle.

Ces lignes représentent également environ **22,7 % du chiffre d'affaires commercial net**. Les exclure du RFM est nécessaire, mais cela réduit fortement la couverture client de l'analyse.

### Doublons

Les **445 doublons exacts** sont supprimés. Leur conservation aurait augmenté artificiellement les quantités, le chiffre d'affaires et parfois la fréquence.

### Retours et annulations

Les quantités négatives représentent environ **2,15 % des lignes brutes**. Après filtrage commercial, **26 894 lignes de retour** totalisent **-884 533 €**.

Elles sont :

- conservées dans le **montant monétaire net** ;
- exclues de la **fréquence** et de la **récence d'achat positif** ;
- exclues de la distribution des paniers de vente.

Cette distinction évite de traiter un retour comme une nouvelle commande tout en conservant son effet économique.

### Prix nuls et négatifs

- **0,68 %** des lignes ont un prix nul : cadeaux, tests ou échantillons possibles ;
- **0,01 %** ont un prix négatif : ces lignes sont considérées comme invalides pour les vues de vente ;
- les frais, ajustements et codes non-produits sont isolés des analyses produit.

### Données déclaratives incomplètes

| Variable | Profils renseignés | Profils manquants | Usage possible |
|---|---:|---:|---|
| Tranche d'âge | 42,7 % | 57,3 % | Adapter le ton, jamais la valeur du client |
| Préférence déclarée | 38,1 % | 61,9 % | Choisir l'angle produit |
| Étape de vie | 22,5 % | 77,5 % | Analyse exploratoire seulement |
| Densité urbaine | 19,8 % | 80,2 % | Trop faible pour piloter seule une campagne |

Seulement **16,3 %** des profils possèdent à la fois une préférence et une tranche d'âge. En revanche, **64,6 %** possèdent au moins l'une des deux. Les deux informations sont donc utilisées indépendamment, avec une modalité « non renseignée ».

## 5. Portrait quantitatif des clients

### Fréquence et fidélité

| Indicateur CRM | Valeur |
|---|---:|
| Nombre médian de commandes | 3 |
| Nombre moyen de commandes | 4,59 |
| 75e percentile | 6 commandes |
| 90e percentile | 11 commandes |
| Maximum observé | 373 commandes |
| Clients avec une seule commande | 28,8 % |
| Clients récurrents | 71,2 % |

La majorité des clients revient, mais près de trois clients sur dix restent au premier achat. Le passage de la première à la deuxième commande constitue donc un enjeu marketing majeur.

La distribution est très asymétrique : quelques profils achètent extrêmement souvent. C'est pourquoi le score de fréquence du RFM utilise des seuils observés **1, 2, 3-4, 5-7 et 8 commandes ou plus**, plutôt que de séparer artificiellement des clients ayant la même fréquence pour former cinq groupes de taille identique.

### Montants et paniers CRM

| Indicateur CRM | Médiane | Moyenne | 90e percentile | Maximum |
|---|---:|---:|---:|---:|
| Total dépensé | 1 597 € | 3 374 € | 8 387 € | 416 441 € |
| Panier moyen client | 561 € | 724 € | 1 480 € | 11 891 € |

La moyenne est nettement supérieure à la médiane : une minorité de très gros clients tire les montants vers le haut. Les graphiques sont donc limités au 99e percentile pour rester lisibles, sans supprimer ces profils de la donnée.

### Récence et ancienneté CRM

| Indicateur | Médiane | Moyenne | 75e percentile | Maximum |
|---|---:|---:|---:|---:|
| Récence | 109 jours | 207 jours | 376 jours | 846 jours |
| Ancienneté | 503 jours | 463 jours | 649 jours | 848 jours |

La moitié des profils CRM a acheté dans les 109 derniers jours, mais un quart dépasse 376 jours d'inactivité. Cette dispersion justifie de distinguer les clients actifs, à risque et perdus.

## 6. Relations entre les comportements

Les corrélations de Spearman décrivent des relations monotones, sans supposer une relation linéaire.

| Relation | Corrélation | Lecture |
|---|---:|---|
| Fréquence / total dépensé | +0,761 | Les achats répétés sont fortement associés à la valeur cumulée |
| Panier moyen / total dépensé | +0,686 | Le niveau de panier contribue aussi fortement à la valeur |
| Récence / fréquence | -0,518 | Les clients les plus récents ont tendance à acheter plus souvent |
| Récence / total dépensé | -0,449 | Une longue inactivité est associée à une valeur plus faible |
| Fréquence / panier moyen | +0,093 | Acheter souvent ne signifie presque pas dépenser davantage par visite |
| Ancienneté / fréquence | +0,438 | Une relation longue donne davantage d'occasions de commander |

La valeur client vient donc de **deux mécanismes distincts** : acheter plus souvent et avoir un panier élevé. Une stratégie unique centrée uniquement sur le panier manquerait les clients fidèles à panier modéré.

Ces corrélations décrivent des associations. Elles ne prouvent pas qu'une relance provoquera un achat.

## 7. Concentration de la valeur

Selon les agrégats CRM utilisés dans l'EDA initiale, **34,3 % des clients génèrent 80 % du chiffre d'affaires client**. Selon le montant net reconstruit depuis les transactions identifiées, cette proportion est d'environ **22,6 %**.

L'écart vient de la divergence entre les deux sources. Dans les deux cas, la conclusion reste la même : la valeur est concentrée et les clients ne doivent pas recevoir le même niveau d'investissement marketing.

Cette concentration justifie :

- une protection active des **Champions** ;
- une réactivation prioritaire des clients **À risque** ayant une forte valeur passée ;
- des dispositifs automatisés et moins coûteux pour les profils à faible valeur ou faible probabilité de retour.

## 8. Produits et catégories

Sur les ventes produit positives, la part du chiffre d'affaires est la suivante :

| Catégorie | Part du CA positif |
|---|---:|
| Visage | 31,9 % |
| Coffret | 20,8 % |
| Corps | 20,7 % |
| Cheveux | 11,3 % |
| Accessoire | 8,1 % |
| Bain | 7,1 % |

**Visage** est la première catégorie, mais elle ne domine pas seule l'activité. **Coffret** et **Corps** représentent ensemble 41,5 % du chiffre d'affaires positif.

Implications marketing :

- utiliser Visage comme porte d'entrée vers Corps ou Coffret ;
- tester les coffrets pour augmenter le panier sans remise uniforme ;
- vérifier les affinités par segment RFM avant de généraliser une recommandation produit ;
- distinguer une préférence déclarée d'un simple historique d'achat.

## 9. Temporalité et croissance

Le chiffre d'affaires commercial net, incluant l'effet des retours, est d'environ **39,09 M€** sur la période disponible. Les ventes positives représentent environ **39,98 M€** avant déduction des retours.

| Année | CA net approximatif | Couverture |
|---|---:|---|
| 2022 | 0,05 M€ | 12 mois |
| 2023 | 0,68 M€ | 12 mois |
| 2024 | 11,38 M€ | 12 mois |
| 2025 | 24,30 M€ | 12 mois |
| 2026 | 14,17 M€ | janvier à juin seulement |

La très forte progression suggère une croissance de l'activité, un élargissement du périmètre de collecte ou les deux. Il serait trompeur de comparer directement 2026, année incomplète, à 2025.

Les mois d'**avril à juin** apparaissent régulièrement parmi les plus élevés, avec un pic en juin 2025. Ce signal peut guider le calendrier de campagne, mais il ne suffit pas à prouver une saisonnalité : la croissance rapide du volume crée un effet de tendance. Une validation correcte comparerait les mois à année et périmètre constants.

## 10. Géographie

La France représente **91,27 %** des profils CRM. Les pays suivants sont très minoritaires : Allemagne 1,83 %, Royaume-Uni 1,51 %, Espagne 0,58 %, Portugal 0,42 % et Belgique 0,40 %.

Cette concentration implique que :

- les résultats décrivent principalement le marché français ;
- les paniers élevés de certains petits pays peuvent être dus à quelques clients professionnels ou atypiques ;
- une moyenne par pays doit toujours être accompagnée de l'effectif ;
- les segments ne doivent pas être présentés comme représentatifs de tous les marchés européens.

Par exemple, l'Irlande affiche un panier transactionnel moyen beaucoup plus élevé, mais seulement **14 clients identifiés**. Ce résultat est un signal à examiner, pas une base suffisante pour une stratégie pays.

## 11. Préférences et âge

Parmi les répondants, les sept préférences sont presque équilibrées, chacune regroupant environ 2 700 à 2 800 profils :

- Peau sensible : 2 815 ;
- Anti-âge : 2 768 ;
- Anti-imperfections : 2 764 ;
- Recherche vegan : 2 749 ;
- Peau mixte : 2 715 ;
- Peau sèche : 2 694 ;
- Sans parfum : 2 682.

Les cinq tranches d'âge renseignées sont également équilibrées, entre 4 233 et 4 392 profils. Aucun segment d'âge ou de préférence n'est naturellement dominant.

Conséquence : ces variables servent à **personnaliser un traitement RFM**, pas à remplacer la segmentation comportementale. Il ne faut pas inventer une profession, un genre, un âge exact ou une préoccupation cutanée quand la donnée manque.

## 12. Pourquoi le RFM est recalculé depuis les transactions

Les agrégats de `customers.csv` ne concordent que partiellement avec les indicateurs reconstruits depuis `transactions.csv` :

| Contrôle | Taux d'accord |
|---|---:|
| Récence exacte | 11,93 % |
| Fréquence exacte | 78,18 % |
| Montant à 0,02 € près | 6,90 % |

Les montants illustrent fortement cet écart :

- moyenne `total_spent` du CRM : **3 374 €** ;
- moyenne monétaire nette reconstruite : environ **793 €** par client présent dans les agrégats transactionnels.

Plusieurs explications sont possibles : historique transactionnel incomplet, périmètres commerciaux différents, règles d'agrégation distinctes ou données provenant de systèmes non synchronisés. Les fichiers seuls ne permettent pas de choisir entre ces hypothèses.

Le projet adopte donc la règle suivante :

- **transactions** pour calculer la récence, la fréquence, le montant et les segments de manière reproductible ;
- **CRM** pour décrire les profils et enrichir les messages ;
- **réconciliation avec l'équipe data** avant toute mise en production.

## 13. Lecture des sept segments RFM

La segmentation couvre **49 184 clients** disposant d'au moins une vente positive identifiée.

| Segment | Clients | Part clients | Part CA net | Lecture métier |
|---|---:|---:|---:|---|
| Champions | 9 334 | 19,0 % | 61,7 % | Récents, fréquents et forte valeur |
| Potentiels fidèles | 9 420 | 19,2 % | 12,5 % | Relation récente à consolider |
| Fidèles | 4 338 | 8,8 % | 8,8 % | Achats réguliers à entretenir |
| À risque | 4 704 | 9,6 % | 8,1 % | Valeur passée élevée mais activité ancienne |
| Occasionnels | 6 574 | 13,4 % | 6,1 % | Potentiel incertain, coût à contrôler |
| Perdus | 10 746 | 21,8 % | 2,0 % | Très inactifs et faible valeur récente |
| Nouveaux prometteurs | 4 068 | 8,3 % | 0,8 % | Très récents, encore peu actifs |

Le contraste principal est net : **19 % des clients Champions portent 61,7 % du montant net**, tandis que **21,8 % de clients Perdus n'en portent que 2 %**. Le RFM sert précisément à traduire cet écart en objectifs, budgets et cadences différents.

## 14. Ce que les données permettent de décider

### Décisions suffisamment étayées

- Prioriser les Champions et les clients À risque.
- Déployer un onboarding visant le deuxième achat.
- Automatiser les contacts destinés aux Occasionnels.
- Limiter les dépenses de reconquête sur les Perdus.
- Adapter l'angle produit lorsque la préférence est déclarée.
- Adapter la forme du message lorsque la tranche d'âge est connue.
- Mesurer chaque campagne avec un groupe témoin.

### Décisions non étayées par ces fichiers

- Choisir un canal individuel : aucune donnée de contact ou de consentement n'est fournie.
- Déduire une profession, un genre, un revenu ou une pathologie.
- Affirmer qu'une campagne cause un achat sans test contrôlé.
- Généraliser les résultats aux marchés hors France.
- Utiliser les agrégats CRM et transactionnels comme s'ils étaient interchangeables.

## 15. Limites et contrôles avant activation

1. **Couverture client incomplète :** 22,77 % des lignes ne sont pas rattachables à un client.
2. **Sources non réconciliées :** les montants et récences CRM divergent fortement des transactions.
3. **Historique en croissance :** les premières années semblent avoir un périmètre beaucoup plus faible.
4. **Biais géographique :** 91,27 % des profils sont français.
5. **Données déclaratives rares :** aucune imputation de préférence ou d'âge ne doit être réalisée.
6. **Valeurs extrêmes :** elles peuvent signaler des profils professionnels ou des anomalies à contrôler.
7. **Conformité :** base légale, consentement applicable, droit d'opposition, durée de conservation et pression commerciale doivent être vérifiés avant tout contact.

## 16. Conclusion

Le projet montre une clientèle hétérogène, une valeur fortement concentrée et un enjeu clair de deuxième achat. Les transactions fournissent la base la plus reproductible pour mesurer le comportement, tandis que le CRM apporte un enrichissement utile mais incomplet.

La stratégie recommandée n'est donc pas de créer des centaines de personas fragiles. Elle consiste à appliquer une logique à trois niveaux :

1. **RFM :** qui prioriser et dans quel objectif ;
2. **Préférence déclarée :** quelle offre présenter ;
3. **Tranche d'âge :** comment formuler le message.

Cette approche reste actionnable malgré les données manquantes, tout en conservant des règles de repli et des garde-fous contre les interprétations non justifiées.
