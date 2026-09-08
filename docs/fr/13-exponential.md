# 13. L'arithmétique Exponential

Solidity ne connaît pas les décimales, et un protocole de prêt manipule surtout des fractions : taux de change, taux d'intérêt, facteurs de collatéral. `ExponentialNoError.sol` fournit la convention utilisée partout.

```solidity
uint constant expScale = 1e18;
uint constant doubleScale = 1e36;

struct Exp { uint mantissa; }
struct Double { uint mantissa; }
```

Un `Exp` est un nombre multiplié par 10¹⁸. Le suffixe `Mantissa` qui traîne dans tout le code — `collateralFactorMantissa`, `borrowRateMantissa`, `exchangeRateMantissa` — signale cette échelle. Un facteur de collatéral de 0,75 est stocké `0.75e18`.

Le type `Double`, à 10³⁶, sert là où deux `Exp` se multiplient et où la précision doit tenir : la distribution du COMP, notamment.

Les fonctions du fichier gèrent le décalage d'échelle que la multiplication introduit. Les noms disent l'ordre des opérations : `mul_ScalarTruncate` multiplie puis tronque, `mul_ScalarTruncateAddUInt` multiplie, tronque, puis ajoute un entier — la forme exacte qu'exigent les calculs du chapitre 4.

Le nom du fichier annonce un choix : *NoError*. Ces opérations ne retournent pas de code d'erreur ; elles échouent par revert en cas de débordement. C'est une évolution par rapport à `Exponential.sol` et au style de `ErrorReporter.sol`, où les erreurs se propageaient comme valeurs de retour — un style que Solidity moderne remplace par les erreurs personnalisées visibles au chapitre 11.

Suite : [limites de ce parcours](14-limites.md).
