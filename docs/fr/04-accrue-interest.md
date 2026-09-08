# 4. accrueInterest et le borrowIndex

Les intérêts ne sont pas calculés en continu : personne ne paie le gaz pour ça. `accrueInterest()` rattrape d'un coup tout le temps écoulé depuis le dernier passage.

```solidity
uint blockDelta = currentBlockNumber - accrualBlockNumberPrior;
Exp memory simpleInterestFactor = mul_(Exp({mantissa: borrowRateMantissa}), blockDelta);
uint interestAccumulated = mul_ScalarTruncate(simpleInterestFactor, borrowsPrior);
uint totalBorrowsNew = interestAccumulated + borrowsPrior;
uint borrowIndexNew  = mul_ScalarTruncateAddUInt(simpleInterestFactor, borrowIndexPrior, borrowIndexPrior);
```

Le taux est **par bloc**, pas par an : le facteur d'intérêt est le taux multiplié par le nombre de blocs écoulés. Si personne n'a touché le marché pendant mille blocs, les mille blocs sont facturés au passage suivant.

L'intérêt est simple sur l'intervalle, composé entre les intervalles : chaque appel repart d'un `totalBorrows` déjà gonflé.

Le `borrowIndex` est l'astuce qui évite de parcourir la liste des emprunteurs. C'est un compteur global qui suit la même croissance. La dette d'un compte se recalcule à la demande :

```solidity
dette = principal * borrowIndex_actuel / borrowIndex_au_moment_de_l_emprunt
```

Chaque emprunteur ne stocke que deux nombres — son principal et l'index qu'il a connu. La mise à jour d'un seul compteur suffit à faire courir les intérêts de tout le monde.

Un garde-fou ferme la fonction : `require(borrowRateMantissa <= borrowRateMaxMantissa, "borrow rate is absurdly high")`.

Suite : [mint et redeem](05-mint-redeem.md).
