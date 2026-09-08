# 7. Le modèle de taux

Aucun humain ne fixe les taux. Ils dérivent du **taux d'utilisation**, la part du pool qui est empruntée :

```solidity
utilizationRate = borrows * BASE / (cash + borrows - reserves)
```

À zéro, tout dort. À un, tout est prêté et plus personne ne peut retirer.

`JumpRateModel` traduit cette utilisation en taux d'emprunt avec deux pentes séparées par un coude, le `kink` :

```solidity
if (util <= kink) {
    return (util * multiplierPerBlock / BASE) + baseRatePerBlock;
} else {
    uint normalRate = (kink * multiplierPerBlock / BASE) + baseRatePerBlock;
    uint excessUtil = util - kink;
    return (excessUtil * jumpMultiplierPerBlock / BASE) + normalRate;
}
```

Sous le coude, le taux monte doucement. Au-dessus, `jumpMultiplierPerBlock` le fait décoller. C'est un mécanisme d'incitation, pas de tarification : un taux qui explose attire des dépôts et pousse au remboursement, ce qui ramène de la liquidité en caisse et protège la capacité de retrait décrite au chapitre 5.

Le taux de rémunération des déposants découle du taux d'emprunt, diminué de la part réservée au protocole :

```
supplyRate = utilisation × borrowRate × (1 − reserveFactor)
```

Le déposant est toujours payé moins que ce que paie l'emprunteur — l'écart couvre la part non prêtée et les réserves.

Suite : [le Comptroller](08-comptroller.md).
