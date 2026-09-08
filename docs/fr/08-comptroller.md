# 8. Le Comptroller, gardien du risque

Un `CToken` sait compter ; il ne sait pas juger. Toute décision qui met en jeu plusieurs marchés revient au Comptroller.

Le motif est constant. Chaque opération sensible appelle son autorisation avant d'agir : `mintAllowed`, `redeemAllowed`, `borrowAllowed`, `repayBorrowAllowed`, `liquidateBorrowAllowed`, `seizeAllowed`, `transferAllowed`. Un retour non nul fait échouer l'opération.

Ce découpage a une conséquence de conception : la logique de risque est **remplaçable sans toucher aux marchés**. Le Comptroller peut être mis à jour, ajouter un plafond ou une pause, sans redéployer un seul cToken ni déplacer un seul dépôt.

Le Comptroller tient aussi la liste des marchés dans lesquels chaque compte est **entré**. Un dépôt ne sert de garantie que si le compte a appelé `enterMarkets` pour ce marché ; `exitMarket` l'en retire, à condition qu'il n'en résulte aucun déficit. Un actif déposé hors de cette liste rapporte des intérêts mais ne garantit rien.

Trois réglages par marché structurent le risque, tous bornés dans le code : le facteur de collatéral (au plus 0,9), le facteur de fermeture (entre 0,05 et 0,9) et la prime de liquidation.

C'est également ici que se distribue le jeton COMP, marché par marché.

Suite : [collatéral et liquidité de compte](09-collateral-liquidite.md).
