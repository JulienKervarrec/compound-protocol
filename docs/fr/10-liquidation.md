# 10. La liquidation

Dès qu'un compte est en `shortfall`, n'importe qui peut rembourser une partie de sa dette et saisir sa garantie. Le liquidateur avance son propre argent ; sa rémunération est une prime.

Deux bornes encadrent l'opération.

Le **facteur de fermeture** limite ce qu'une liquidation peut rembourser en une fois :

```solidity
uint maxClose = mul_ScalarTruncate(Exp({mantissa: closeFactorMantissa}), borrowBalance);
```

Un compte n'est donc pas soldé d'un coup. Le code borne ce facteur entre 0,05 et 0,9. Le but est de ramener le compte au-dessus de l'eau sans le liquider intégralement sur un écart de prix passager.

La **prime de liquidation** fixe le gain. Le nombre de parts saisies vaut :

```
seizeTokens = montantRemboursé × (liquidationIncentive × prixEmprunté)
              / (prixCollatéral × exchangeRate)
```

Avec une prime de 1,08, rembourser 100 dollars de dette rapporte 108 dollars de garantie. Les 8 dollars sortent de la poche de l'emprunteur.

Le liquidateur reçoit des **cTokens**, pas l'actif sous-jacent — `seize` transfère des parts d'un compte à l'autre à l'intérieur du marché de la garantie. Il lui reste à appeler `redeem` s'il veut l'actif.

Rien n'oblige personne à liquider. Le protocole ne fait que rendre l'opération rentable, et compte sur des robots pour qu'elle ait lieu. Quand la prime ne couvre plus le gaz et le glissement, les liquidations n'arrivent pas.

Suite : [le motif Fresh](11-motif-fresh.md).
