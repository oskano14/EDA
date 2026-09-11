pandas>=2.3,<4mport Path  & Co attributionionion

## Question du CMO

> Quels canaux fonctionnent réellement et comment faut-il arbitrer le budget ?

L'analyse couvre 6 campagnes, 1 080 897 touchpoints et 94 366 parcours
convertis. Un parcours comporte 5,03 contacts en moyenne et 99,0 % des
parcours convertis sont multicanaux.

## Performance des campagnes

| Campagne | Canal principal | Conversion | CPA | CAC | ROAS |
|---|---|---:|---:|---:|---:|
| Winter Promo | Display | 55,3 % | 9,45 € | 669,75 € | 5,35 |
| Black Friday | Display | 69,7 % | 10,96 € | 788,31 € | 4,65 |
| Back to School | Social | 51,7 % | 12,07 € | 605,54 € | 4,56 |
| Spring Launch | Display | 76,5 % | 18,40 € | 573,11 € | 4,50 |
| Valentine | Display | 58,6 % | 16,34 € | 955,47 € | 3,45 |
| Summer Sale | Display | 50,1 % | 17,84 € | 2 024,77 € | 2,74 |

Le classement change selon l'objectif. Winter Promo maximise le ROAS et
minimise le CPA, tandis que Spring Launch possède le meilleur taux de
conversion et le CAC le plus faible. Un bon CPA ne garantit donc pas un bon CAC.

Le champ `primary_channel` décrit l'orientation d'une campagne, pas la
performance propre du canal. Cinq campagnes sur six sont étiquetées display,
alors que display ne ferme directement aucune conversion dans les parcours.

## Attribution par canal

| Canal | Part du CA first touch | Part du CA linéaire | Part du CA last touch | Part du coût |
|---|---:|---:|---:|---:|
| Display | 41,1 % | 17,3 % | 0,0 % | 0,1 % |
| Social | 40,9 % | 17,1 % | 0,0 % | 12,3 % |
| Affiliation | 3,7 % | 15,7 % | 28,9 % | 83,7 % |
| Retargeting | 3,8 % | 14,6 % | 24,2 % | 0,1 % |
| Direct | 3,3 % | 12,2 % | 17,4 % | 0,0 % |
| Search payant | 3,7 % | 11,8 % | 15,4 % | 3,5 % |
| Email | 3,6 % | 11,5 % | 14,1 % | 0,2 % |

Le modèle choisi inverse la lecture : display et social créent principalement
la découverte, tandis que l'affiliation, le retargeting, le direct, le search
payant et l'email ferment les ventes. Une décision fondée uniquement sur le
last touch sous-investirait donc le haut du parcours.

## Recommandation

1. Utiliser provisoirement l'attribution multi-touch linéaire, complétée par les
   vues first et last touch pour interpréter le rôle des canaux.
2. Auditer l'affiliation avant de maintenir ses 83,7 % du coût observé : elle
   reçoit 28,9 % du CA en last touch et 15,7 % en attribution linéaire.
3. Préserver un budget test pour display et social, puis mesurer leur apport par
   holdout géographique ou temporel.
4. Tester email et retargeting avec groupes témoins avant de conclure à partir
   de leurs ROAS directs exceptionnellement élevés.
5. Ne réallouer aucun budget majeur avant réconciliation de la définition du CA.

## Limites

- Les coûts et les conversions concordent entre les sources.
- Le CA déclaré dans `campaigns.csv` atteint 5,56 M€, contre 18,40 M€ pour les
  factures converties reconstruites depuis `transactions.csv`.
- First touch, last touch et linéaire répartissent le crédit mais ne démontrent
  pas l'effet causal d'un canal.
- Les touchpoints et les clics sont des métriques de vanité s'ils ne sont pas
  reliés à un coût, une conversion ou un revenu incrémental.

Les calculs reproductibles et leurs tests figurent dans
`03_kpis_attribution_multicanal_lumina.ipynb`.