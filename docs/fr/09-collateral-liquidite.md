# 9. Facteur de collatéral et liquidité de compte

Un dépôt ne garantit jamais sa pleine valeur. Le **facteur de collatéral** de chaque marché dit quelle fraction compte, et le code plafonne cette fraction à 0,9 :

```solidity
uint internal constant collateralFactorMaxMantissa = 0.9e18;
```

Un actif volatil reçoit un facteur bas, un stablecoin un facteur haut. La marge absorbe les mouvements de prix entre deux blocs.

`getHypotheticalAccountLiquidityInternal` parcourt tous les marchés où le compte est entré et accumule deux sommes :

- `sumCollateral` — pour chaque dépôt : parts × taux de change × prix × facteur de collatéral ;
- `sumBorrowPlusEffects` — pour chaque dette : montant × prix, **sans** aucun facteur.

Les paramètres `redeemTokens` et `borrowAmount` ajoutent l'opération envisagée à la seconde somme, ce qui rend la même fonction utilisable pour un retrait comme pour un emprunt.

La comparaison donne l'un ou l'autre, jamais les deux :

- `liquidity` = ce qu'il reste à emprunter, si le collatéral l'emporte ;
- `shortfall` = le déficit, si les dettes l'emportent.

**Un `shortfall` non nul rend le compte liquidable.** C'est la seule condition, et elle ne dépend d'aucun jugement extérieur.

Toutes les valeurs passent par l'oracle de prix. Un prix faux fausse cette somme, et rien dans ces contrats ne peut le rattraper — le chapitre 14 y revient.

Suite : [la liquidation](10-liquidation.md).
