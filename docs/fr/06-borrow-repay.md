# 6. borrow et repayBorrow

`borrow(amount)` sort l'actif du pool vers l'emprunteur. Aucune échéance, aucun calendrier : la dette court tant qu'elle n'est pas remboursée, et grossit à chaque bloc.

Le contrôle sérieux est dans `borrowAllowed` du Comptroller, qui simule l'emprunt avant de l'autoriser :

```solidity
(Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(borrower, CToken(cToken), 0, borrowAmount);
if (shortfall > 0) { ... }
```

Le mot *hypothetical* est le bon : la vérification porte sur l'état qu'aurait le compte **après** l'opération, pas sur son état actuel.

`repayBorrow` rembourse. Une variante, `repayBorrowBehalf`, permet de rembourser la dette de quelqu'un d'autre — sans autorisation de sa part, puisque personne n'est lésé par un remboursement. C'est cette fonction que la liquidation réutilise.

Le montant `type(uint).max` est traité comme « tout rembourser », ce qui évite la course entre le calcul du solde exact et son évolution d'ici à l'inclusion du bloc.

Le remboursement diminue `totalBorrows` et augmente `totalCash`. Le taux de change des cTokens ne bouge pas : les intérêts avaient déjà été comptés au fil des `accrueInterest`.

Suite : [le modèle de taux](07-modele-de-taux.md).
