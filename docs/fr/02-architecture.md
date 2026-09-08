# 2. Architecture du dépôt

Deux contrats font le protocole, et ils ont des rôles nettement séparés.

`CToken.sol` (1156 lignes) est la **comptabilité d'un marché** : un seul actif, ses dépôts, ses emprunts, ses intérêts. Il ne sait rien des autres marchés. Ses déclinaisons `CErc20.sol` et `CEther.sol` ne changent que la façon d'encaisser l'actif sous-jacent.

`Comptroller.sol` (1471 lignes) est le **gardien du risque**. Il connaît tous les marchés, les prix, les facteurs de collatéral, et autorise ou refuse chaque opération. Un `CToken` lui demande la permission avant d'agir.

Autour :

- `InterestRateModel.sol` et ses implémentations (`JumpRateModel`, `WhitePaperInterestRateModel`) calculent le taux d'emprunt.
- `Unitroller.sol` et `CErc20Delegator.sol` sont les proxys qui rendent le code remplaçable.
- `ExponentialNoError.sol` fournit l'arithmétique à virgule fixe.
- `PriceOracle.sol` déclare la source des prix ; `Governance/` contient le jeton COMP et le Governor.

`ComptrollerG7.sol` est une version antérieure conservée pour l'historique : ne pas la confondre avec la version active.

Suite : [le cToken et le taux de change](03-ctoken.md).
