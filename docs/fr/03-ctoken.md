# 3. Le cToken et le taux de change

Le nombre de cTokens d'un déposant ne change jamais tout seul. Ce qui change, c'est ce qu'un cToken vaut :

```solidity
exchangeRate = (totalCash + totalBorrows - totalReserves) / totalSupply
```

Les trois termes du numérateur disent tout du protocole. `totalCash` est ce qui dort dans le contrat. `totalBorrows` est ce qui est dehors, chez les emprunteurs — cette somme grossit avec les intérêts. `totalReserves` est la part mise de côté pour le protocole, qui n'appartient pas aux déposants et se soustrait donc.

Quand un emprunteur paie des intérêts, `totalBorrows` augmente, `totalSupply` ne bouge pas : le taux de change monte, et chaque déposant est payé sans qu'aucun transfert n'ait lieu.

Le cas du premier dépôt est traité à part — avec `totalSupply` à zéro, la division est impossible, donc le contrat retourne `initialExchangeRateMantissa`, une constante fixée au déploiement.

Deux versions de chaque lecture coexistent partout dans le code : `exchangeRateStored()` lit la dernière valeur écrite, `exchangeRateCurrent()` déclenche d'abord le calcul des intérêts. La première est gratuite mais peut être périmée ; la seconde est exacte mais écrit en stockage.

Suite : [accrueInterest et le borrowIndex](04-accrue-interest.md).
