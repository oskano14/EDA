ngineio>=4.12.2->locust>=2.29->-r requirements.txt (line 18)) (1.1.0)

## Ce que demande le cours

Le TP3 ne demande pas de refaire l'EDA ou la segmentation. Il demande de **sélectionner les résultats qui conduisent à une décision**.

Le cours distingue quatre niveaux :

- **Observation :** ce que montrent objectivement les données ;
- **Insight :** ce que cette observation signifie pour le problème étudié ;
- **Hypothèse :** une explication possible, encore à vérifier ;
- **Recommandation :** une action précise et mesurable.

La narration recommandée suit la logique :

> What? → So what? → Now what?

Autrement dit : **que montrent les données, pourquoi est-ce important, que faut-il faire ?**

## Livrables attendus

### Partie I - Faire émerger un insight

Le rendu doit contenir :

- une problématique business ;
- cinq faits marquants sans interprétation ;
- deux ou trois insights ;
- une hypothèse de recommandation.

### Partie II - Transformer l'analyse en Data Story

Le CMO doit pouvoir comprendre le problème et décider en **5 minutes**, avec **5 slides maximum** :

1. problématique business : un contexte, une tension, une question ;
2. insights clés : deux ou trois enseignements maximum ;
3. preuve data : deux ou trois visualisations maximum ;
4. recommandation : une action prioritaire, une cible et un KPI ;
5. impact : un impact estimé et une décision demandée.

Le pitch ne doit pas raconter la démarche d'analyse. Il doit défendre une recommandation.

# Proposition pour Lumina & Co

## Audience et décision

- **Audience :** CMO de Lumina & Co.
- **Ce qui l'intéresse :** protéger la valeur client sans augmenter inutilement la pression commerciale.
- **Décision attendue :** autoriser un pilote contrôlé de réactivation sur les clients À risque.

## Problématique business

> **Comment réactiver les clients à forte valeur devenus inactifs sans dépenser sur des profils ayant une faible probabilité de retour ?**

Cette problématique est plus convaincante qu'une présentation générale des sept segments : elle contient une tension, désigne un enjeu de valeur et appelle une décision précise.

## Cinq faits marquants

Ces phrases sont des observations. Elles ne cherchent pas encore à expliquer les causes.

1. **49 184 clients** disposent d'au moins une vente positive identifiable et peuvent être segmentés.
2. Les **Champions représentent 19,0 % des clients et 61,7 % du montant net**.
3. Les **4 704 clients À risque représentent 9,6 % des clients et 8,1 % du montant net**.
4. Les clients À risque ont une **récence médiane de 454 jours**, malgré une fréquence médiane de **4 commandes**.
5. Les **10 746 clients Perdus représentent 21,8 % des clients, mais seulement 2,0 % du montant net**.

Fait complémentaire à garder en réserve : **28,8 % des 50 295 profils CRM n'ont qu'une commande enregistrée**.

## Trois insights

### Insight 1 - La valeur exige une allocation différenciée

- **Observation :** 19,0 % des clients Champions portent 61,7 % du montant net.
- **Interprétation :** la valeur est fortement concentrée ; une pression commerciale uniforme répartit mal les moyens.
- **Conséquence business :** les budgets et niveaux de service doivent dépendre du segment RFM.

### Insight 2 - Les clients À risque constituent l'opportunité immédiate

- **Observation :** 4 704 clients ayant déjà commandé quatre fois en médiane n'ont pas acheté depuis 454 jours en médiane.
- **Interprétation :** leur problème n'est pas l'absence d'historique, mais la rupture d'une relation auparavant active.
- **Conséquence business :** une campagne de réactivation ciblée est plus défendable qu'une remise envoyée à toute la base.

### Insight 3 - Les clients Perdus ne doivent pas absorber le budget prioritaire

- **Observation :** 21,8 % des clients Perdus ne représentent que 2,0 % du montant net.
- **Interprétation :** leur poids démographique est élevé mais leur valeur économique observée est faible.
- **Conséquence business :** ils doivent être exclus des médias payants et limités à une campagne de permission peu coûteuse.

## Hypothèse de recommandation

> Une relance en deux temps, personnalisée par préférence déclarée ou dernière catégorie achetée, pourrait générer davantage de réactivations incrémentales chez les clients À risque qu'une communication CRM standard.

Il s'agit d'une hypothèse, pas d'un résultat démontré. Elle doit être testée avec un groupe témoin.

# Storyboard des cinq slides

## Slide 1 - La valeur passée s'éloigne

**Titre-message :** `4 704 clients de valeur n'ont pas acheté depuis plus d'un an`

- Contexte : la segmentation porte sur 49 184 clients au 30 juin 2026.
- Tension : le segment À risque a déjà commandé quatre fois en médiane, mais sa récence médiane atteint 454 jours.
- Question : comment récupérer cette valeur sans financer une reconquête générale ?

**À l'oral :** moins de 30 secondes.

## Slide 2 - Tous les clients ne justifient pas le même effort

**Titre-message :** `La valeur est concentrée, mais le risque est actionnable`

Afficher trois enseignements :

- Champions : 19,0 % des clients, 61,7 % du montant net ;
- À risque : 9,6 % des clients, 8,1 % du montant net ;
- Perdus : 21,8 % des clients, 2,0 % du montant net.

Le message n'est pas « voici sept segments ». Le message est : **investir uniformément ignore la différence de valeur et de potentiel**.

## Slide 3 - La preuve data

**Titre-message :** `Les clients À risque combinent valeur passée et longue inactivité`

Deux visualisations suffisent :

1. **Barres horizontales** comparant la part des clients et la part du montant net par segment ;
2. **Nuage récence × montant** mettant en évidence le groupe À risque.

Chaque graphique doit comporter :

- un titre qui énonce la conclusion ;
- la source `customers.csv` / `transactions.csv` ;
- la période, arrêtée au 30 juin 2026 ;
- une légende lisible ;
- une annotation sur le segment À risque.

Éviter le camembert à sept catégories, les barres 3D et les tableaux complets du notebook.

## Slide 4 - Tester une réactivation ciblée pendant 30 jours

**Titre-message :** `Un pilote contrôlé peut mesurer la valeur réellement récupérée`

- **Cible :** les 4 704 clients À risque, sous réserve des règles de contact.
- **Action J0 :** rappel personnalisé fondé sur la préférence déclarée ou le dernier univers acheté.
- **Action J+7 :** avantage limité pour les non-convertis.
- **Contrôle :** groupe témoin aléatoire sans campagne.
- **Arrêt :** conversion, opposition ou fin du test.
- **KPI principal :** taux de réactivation incrémental à 30 jours.
- **KPI secondaires :** marge incrémentale, panier, désabonnement et plaintes.

## Slide 5 - Décider sur un résultat incrémental

**Titre-message :** `Un gain incrémental de 5 % représenterait environ 235 clients réactivés`

Scénario illustratif :

- population : 4 704 clients ;
- hypothèse de gain incrémental : 5 % ;
- résultat correspondant : environ **235 réactivations supplémentaires** ;
- à 137 € de valeur moyenne historique par commande : environ **32 000 € de chiffre d'affaires brut**.

Ce chiffre n'est pas une prévision garantie. C'est un **seuil de scénario** fondé sur la valeur historique, à confirmer par le test et à convertir en marge après coût de campagne et remises.

**Décision demandée au CMO :**

> Valider le pilote de 30 jours, son groupe témoin et le seuil de succès avant généralisation.

# Trame du pitch oral

## 0:00 à 0:30 - Problème

« Lumina compte 4 704 clients À risque. Ils ont commandé quatre fois en médiane, mais leur dernier achat remonte à 454 jours. La question n'est pas de contacter davantage toute la base : c'est de récupérer cette valeur sans dépenser sur les profils les moins prometteurs. »

## 0:30 à 2:00 - Ce que montrent les données

« La valeur est très concentrée : 19 % de Champions portent 61,7 % du montant net. À l'inverse, 21,8 % de clients Perdus n'en portent que 2 %. Entre les deux, le segment À risque représente 9,6 % des clients et 8,1 % du montant net : il combine une valeur passée réelle et une longue inactivité. »

## 2:00 à 3:15 - Ce que cela signifie

« Une campagne uniforme traite de la même manière des potentiels très différents. Les clients À risque ont déjà démontré leur capacité à acheter. Ils constituent donc une cible plus crédible pour une réactivation que les clients Perdus, tout en laissant les Champions dans une logique de fidélisation. »

## 3:15 à 4:30 - Ce qu'il faut faire

« Je recommande un pilote de 30 jours sur les clients À risque : un rappel personnalisé à J0, un avantage limité à J+7, et un groupe témoin. Le KPI principal sera la réactivation incrémentale, complétée par la marge et le désabonnement. »

## 4:30 à 5:00 - Décision

« À titre de scénario, cinq points de réactivation incrémentale représenteraient environ 235 clients et 32 000 euros de chiffre d'affaires brut sur une commande moyenne historique. Je demande la validation du pilote, du groupe témoin et du seuil de succès avant toute généralisation. »

# Points de vigilance

- Ne pas dire que la campagne **causera** la réactivation avant le test.
- Ne pas présenter les 32 000 € comme une prévision certaine.
- Ne pas mélanger les agrégats CRM et transactionnels sans nommer la source.
- Ne pas dépasser trois messages clés ou trois visualisations.
- Ne pas afficher de chiffre impossible à expliquer oralement.
- Vérifier la base légale, le consentement applicable et le droit d'opposition avant l'envoi.
- Utiliser la préférence et l'âge uniquement lorsqu'ils sont renseignés ; ne rien déduire à partir du produit acheté.
