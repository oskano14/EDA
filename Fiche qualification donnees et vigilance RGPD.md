# Fiche de qualification des données et de vigilance RGPD

## 1. Périmètre

| Élément | Information | Statut |
| --- | --- | --- |
| Organisation | Lumina & Co | Connu |
| Jeux de données analysés | `customers.csv` et `transactions.csv` | Connu |
| Système source | CRM et historique transactionnel | Indiqué dans le sujet |
| Date d'extraction | Non renseignée | À confirmer |
| Responsable de traitement | Non renseigné | À confirmer |
| Responsable interne des données | Non renseigné | À confirmer |
| Hébergement et localisation | Non renseignés | À confirmer |

Cette fiche porte sur l'usage analytique réalisé pendant le Jour 1. Elle ne
vaut pas validation juridique d'une activation marketing réelle.

## 2. Qualification des données

| Données | Exemples | Famille marketing | Qualification RGPD | Source |
| --- | --- | --- | --- | --- |
| Identifiant client | `customer_id` | Identifiant CRM | Donnée personnelle indirecte ou pseudonymisée | First-party |
| Localisation | `country`, `urban_density` | Sociodémographique | Donnée personnelle | First-party / déclarative à confirmer |
| Profil déclaré | `age_bracket`, `life_stage` | Sociodémographique | Donnée personnelle | Zero-party présumée |
| Préférences | `declared_preference` | Déclarative | Donnée personnelle | Zero-party présumée |
| Historique d'achat | Produits, dates, quantités, prix | Transactionnelle | Donnée personnelle lorsqu'elle est liée à un client | First-party |
| Indicateurs clients | Récence, fréquence, montant, panier moyen | Comportementale et transactionnelle | Profil dérivé d'une donnée personnelle | Calcul interne |

Aucune catégorie particulière de données au sens de l'article 9 du RGPD
n'apparaît explicitement dans les fichiers. Une préférence cosmétique pourrait
toutefois devenir sensible si elle permettait d'inférer un état de santé. Cette
possibilité doit être vérifiée avant toute activation individuelle.

## 3. Finalités envisagées

- contrôler la qualité et la cohérence des données CRM ;
- comprendre les comportements d'achat ;
- construire une segmentation RFM ;
- identifier des groupes utiles à la personnalisation marketing ;
- produire des analyses agrégées pour guider les décisions du CMO.

Une réutilisation pour une autre finalité doit faire l'objet d'une vérification
de compatibilité et, si nécessaire, d'une nouvelle information des personnes.

## 4. Base légale et consentement

Les fichiers ne permettent pas de déterminer la base légale ni de prouver le
consentement. Les points suivants doivent être validés avant une campagne :

- l'exécution du contrat peut couvrir le traitement nécessaire des achats ;
- l'intérêt légitime peut éventuellement couvrir certaines analyses internes,
  sous réserve d'un test de mise en balance documenté ;
- la prospection électronique B2C nécessite généralement un consentement, sauf
  exception applicable aux clients existants pour des produits analogues ;
- la preuve du consentement, sa date, sa finalité et son retrait doivent être
  traçables ;
- les personnes doivent pouvoir s'opposer facilement à la prospection.

## 5. Qualité, biais et limites observés

| Constat | Résultat | Risque ou conséquence |
| --- | ---: | --- |
| Transactions sans `customer_id` | 22,77 % | Attribution et segmentation client impossibles |
| Identifiants de transaction absents du référentiel | 90 | Intégrité référentielle incomplète |
| Profils du référentiel sans transaction | 983 | Profils potentiellement inactifs ou incomplets |
| Clients situés en France | 91,3 % | Biais géographique important |
| Remplissage des variables zero-party | 19,8 % à 42,7 % | Personnalisation déclarative peu représentative |
| Doublons exacts dans les transactions | 445 | Risque de double comptage avant nettoyage |

Les données manquantes ne doivent pas être imputées par des préférences ou des
caractéristiques inventées. Les segments peu nombreux ne doivent pas être
interprétés comme représentatifs sans contrôle des effectifs.

## 6. Minimisation et sécurité

- conserver uniquement les colonnes nécessaires à l'objectif de l'analyse ;
- ne pas publier les CSV bruts sur GitHub ;
- limiter l'accès aux personnes participant au projet ;
- ne pas tenter de réidentifier les clients ;
- privilégier les résultats agrégés dans les restitutions ;
- masquer ou regrouper les catégories géographiques à très faible effectif ;
- protéger les exports et supprimer les fichiers temporaires devenus inutiles ;
- documenter les tables dérivées afin de pouvoir appliquer une suppression.

## 7. Conservation et droits des personnes

Les durées exactes ne peuvent pas être déduites des fichiers. Avant tout usage
opérationnel, il faut confirmer :

- la durée de conservation associée à chaque finalité ;
- les règles d'archivage, d'anonymisation et de suppression ;
- la procédure d'accès, de rectification, d'opposition et d'effacement ;
- la propagation d'une demande aux tables dérivées et aux sauvegardes ;
- le délai et le responsable chargés de répondre aux demandes.

Les valeurs pédagogiques du support de cours (36 mois, 13 mois et un mois) ne
doivent pas être appliquées indistinctement : leur portée dépend de la finalité,
du type de donnée et du contexte juridique.

## 8. Décision pour le projet

Les données sont utilisables pour une EDA et une segmentation exploratoire dans
un environnement local contrôlé. Une utilisation réelle pour personnaliser des
campagnes reste conditionnée à la validation de l'origine des données, de la
base légale, du consentement applicable, des durées de conservation et des
procédures d'exercice des droits.

### Contrôles avant activation

- [ ] Identifier le responsable de traitement et le propriétaire des données.
- [ ] Documenter l'origine et la date d'extraction de chaque fichier.
- [ ] Confirmer la base légale pour chaque finalité.
- [ ] Vérifier la preuve du consentement ou l'exception applicable.
- [ ] Valider les durées de conservation et les règles de suppression.
- [ ] Vérifier les droits d'accès aux données et aux exports.
- [ ] Appliquer les demandes d'opposition ou d'effacement aux segments produits.
- [ ] Faire valider l'activation par la personne responsable du RGPD.
