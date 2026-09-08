# 11. Le motif Fresh

Une convention traverse tout `CToken.sol` et mérite un chapitre à elle seule. Chaque opération existe en deux fonctions : `mintInternal` et `mintFresh`, `borrowInternal` et `borrowFresh`, et ainsi de suite.

La partie `Internal` appelle `accrueInterest()`, puis délègue à la partie `Fresh`. Celle-ci commence par vérifier que le travail a bien été fait :

```solidity
if (accrualBlockNumber != getBlockNumber()) {
    revert MintFreshnessCheck();
}
```

Le contrôle peut sembler superflu — la fonction appelante vient d'appeler `accrueInterest`. Il ne l'est pas : il garantit qu'**aucun chemin d'exécution** ne peut atteindre la logique métier sur des chiffres périmés. Toute nouvelle fonction ajoutée plus tard bute sur la même barrière.

L'enjeu est concret. Émettre des cTokens sur un taux de change qui n'inclut pas les derniers blocs d'intérêts crédite trop de parts au déposant, aux frais de tous les autres.

Le même contrat applique aussi le motif *checks-effects-interactions*, marqué par un commentaire explicite au milieu de `accrueInterest` :

```
/////////////////////////
// EFFECTS & INTERACTIONS
// (No safe failures beyond this point)
```

Tout ce qui peut échouer proprement est placé avant ; ensuite, seules les écritures et les appels externes.

Suite : [Unitroller et les proxys](12-unitroller-proxy.md).
