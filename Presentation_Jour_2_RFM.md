# Jour 2 - Lumina & Co RFM Segmentation

## 1. Notre objectif

Le CMO envoyait les mêmes communications à tous les clients.

Notre objectif était de répondre à deux questions :

- qui sont les clients de Lumina & Co ?
- quelle action marketing proposer à chaque groupe ?

Nous avons utilisé la méthode **RFM**, qui classe les clients selon leur
comportement d'achat.

> À l'oral : nous cherchons à remplacer une communication unique par des
> actions adaptées à la valeur, à l'activité et au risque de perte des clients.

---

## 2. Préparation des données

Nous sommes partis de `transactions.csv` après avoir :

- supprimé les 445 doublons exacts ;
- exclu les transactions sans identifiant client ;
- exclu les prix négatifs et les écritures non commerciales ;
- conservé les retours pour calculer un montant net ;
- fixé la date de référence au **30 juin 2026**, dernière date du dataset.

La segmentation couvre **49 184 clients** ayant au moins une vente positive.

> À l'oral : nous n'avons pas utilisé la date du jour, car cela aurait augmenté
> artificiellement la récence de tous les clients.

---

## 3. Calcul du RFM

Pour chaque client, nous avons calculé :

- **Récence (R)** : nombre de jours depuis le dernier achat ;
- **Fréquence (F)** : nombre de factures distinctes ;
- **Montant (M)** : somme nette dépensée, retours compris.

Chaque dimension reçoit un score de **1 à 5** :

- R = 5 signifie un achat très récent ;
- F = 5 signifie au moins 8 commandes ;
- M = 5 correspond aux 20 % de clients ayant le montant net le plus élevé.

Les seuils de récence observés sont 56, 136, 308 et 561 jours. Ceux du montant
sont 77,63 €, 171,36 €, 328,97 € et 683,35 €.

> À l'oral : pour la fréquence, 29,7 % des clients ont une seule commande. Nous
> avons gardé les ex aequo ensemble plutôt que de les séparer artificiellement.

---

## 4. Comment avons-nous segmenté ?

Nous avons d'abord conservé les profils extrêmes proposés dans le cours :

- **Champions** : R, F et M élevés ;
- **Nouveaux prometteurs** : récents, mais encore peu fréquents et peu dépensiers ;
- **À risque** : anciens, mais historiquement fréquents et valorisés ;
- **Perdus** : anciens, peu fréquents et à faible valeur.

Le groupe intermédiaire était trop large. Nous l'avons divisé en :

- **Fidèles** : fréquence et valeur élevées ;
- **Potentiels fidèles** : assez récents avec plusieurs commandes ;
- **Occasionnels** : profils irréguliers ne correspondant pas aux autres règles.

Nous obtenons ainsi **sept segments exhaustifs**, chaque client appartenant à un
seul segment.

---

## 5. Résultats principaux

| Segment | Clients | Part de la base | Part du CA net |
| --- | ---: | ---: | ---: |
| Champions | 9 334 | 19,0 % | 61,7 % |
| Potentiels fidèles | 9 420 | 19,2 % | 12,5 % |
| À risque | 4 704 | 9,6 % | 8,1 % |
| Fidèles | 4 338 | 8,8 % | 8,8 % |
| Occasionnels | 6 574 | 13,4 % | 6,1 % |
| Perdus | 10 746 | 21,8 % | 2,0 % |
| Nouveaux prometteurs | 4 068 | 8,3 % | 0,8 % |

Deux constats dominent :

- les Champions représentent 19 % des clients, mais 61,7 % du CA net ;
- les Perdus représentent 21,8 % des clients, mais seulement 2 % du CA net.

---

## 6. Recommandations et priorité

1. **À risque** : relance personnalisée, car leur valeur historique est menacée.
2. **Champions** : programme VIP et accès anticipé, sans remise systématique.
3. **Potentiels fidèles** : nurturing et vente croisée.
4. **Nouveaux prometteurs** : onboarding pour déclencher le deuxième achat.
5. **Fidèles** : fidélité et rappels de réapprovisionnement.
6. **Occasionnels** : communications automatisées à faible coût.
7. **Perdus** : exclusion des campagnes payantes coûteuses.

> Conclusion orale : la segmentation permet d'investir davantage là où la
> valeur ou le risque sont élevés, et de réduire les dépenses sur les clients
> ayant peu de potentiel observable.

---

## Limites à signaler

- Les agrégats de `customers.csv` concordent mal avec ceux reconstruits depuis
  les transactions ; les scores reposent donc uniquement sur les transactions.
- Le RFM décrit le passé, mais ne prédit pas avec certitude la réponse future.
- Les préférences déclarées ne sont renseignées que pour environ 38 % des clients.
- Toute activation marketing doit respecter la base légale et le droit
  d'opposition des clients.
