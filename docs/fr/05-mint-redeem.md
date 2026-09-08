# 5. mint et redeem : fournir et retirer

`mint` dépose l'actif sous-jacent et émet des cTokens :

```
cTokens émis = montant déposé / exchangeRate
```

`redeem` fait l'inverse. Il existe en deux entrées, et la distinction est pratique : `redeem(cTokens)` rend un nombre de parts, `redeemUnderlying(montant)` vise un montant d'actif. La seconde évite au front-end de calculer un taux de change qui aura changé au moment de l'inclusion.

Les deux opérations passent par le Comptroller avant d'agir. `mintAllowed` vérifie surtout que le marché existe et n'est pas en pause. `redeemAllowed` fait davantage : retirer une garantie peut rendre un emprunt insuffisamment couvert, donc il simule le retrait.

```solidity
(Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(redeemer, CToken(cToken), redeemTokens, 0);
if (shortfall > 0) { ... }
```

Un retrait qui créerait un déficit est refusé.

Une limite structurelle apparaît ici : on ne peut retirer que ce qui est en caisse. Si tout l'actif est emprunté, `totalCash` est à zéro et le retrait échoue, quelle que soit la valeur des parts détenues. C'est le taux d'utilisation du chapitre 7 qui pousse à ce que cela n'arrive pas.

Suite : [borrow et repayBorrow](06-borrow-repay.md).
