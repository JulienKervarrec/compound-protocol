# 12. Unitroller et les proxys

Le protocole détient les fonds de ses utilisateurs et doit pourtant pouvoir corriger son code. Deux proxys règlent ce conflit, sur le même principe.

`Unitroller` est le proxy du Comptroller. Il détient le stockage — les marchés, les facteurs de collatéral, les positions — et ne fait que déléguer :

```solidity
fallback() payable external {
    (bool success, ) = comptrollerImplementation.delegatecall(msg.data);
    ...
}
```

`delegatecall` exécute le code de l'implémentation **dans le contexte du proxy**. Remplacer l'implémentation change le comportement sans déplacer un seul octet de stockage. L'adresse que tout le monde connaît reste l'Unitroller.

`CErc20Delegator` applique la même mécanique à un marché : la logique vit dans un `CErc20Delegate` interchangeable.

Le passage d'une implémentation à l'autre se fait en deux temps : la nouvelle doit s'accepter elle-même via `_become`. Une adresse posée par erreur ne prend donc pas la main.

La contrepartie est double, et elle est structurelle. D'une part, le stockage doit rester compatible d'une version à l'autre — d'où `ComptrollerStorage.sol`, dont l'ordre des variables ne peut pas être modifié. D'autre part, **quiconque peut changer l'implémentation peut changer les règles** : c'est le pouvoir que la gouvernance encadre, avec `Timelock.sol` qui impose un délai entre le vote et l'exécution.

Suite : [l'arithmétique Exponential](13-exponential.md).
