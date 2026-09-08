# 1. Présentation

Compound est un marché monétaire sur Ethereum. On y dépose un actif pour le prêter, on y emprunte un autre actif en laissant le premier en garantie. Il n'y a pas de contrepartie nommée, pas de durée, pas d'échéance : chaque actif a un pool commun, et les taux se recalculent en continu selon le rapport entre ce qui est emprunté et ce qui dort.

Deux idées portent tout le protocole.

La première est le **cToken**. Déposer 100 DAI ne crédite pas un solde de 100 : cela émet des cDAI, une part du pool. Le nombre de parts ne bouge plus ; c'est leur valeur qui monte à mesure que les intérêts entrent. Les intérêts ne sont donc jamais distribués — ils se lisent dans un taux de change.

La seconde est la **sur-collatéralisation**. Un emprunt vaut toujours moins que la garantie déposée. Si le prix bouge et que la garantie devient insuffisante, n'importe qui peut rembourser une partie de la dette à la place de l'emprunteur et saisir sa garantie avec une prime.

Ce dépôt contient la version 2 du protocole : environ 7 000 lignes de Solidity, deux contrats centraux et un ensemble de modules autour.

Suite : [architecture du dépôt](02-architecture.md).
