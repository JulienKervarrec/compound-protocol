# 14. Limites de ce parcours

Ce parcours est **documentaire**. Rien n'a été installé, compilé ni exécuté pour le rédiger. Tout provient de la lecture des contrats et de leurs commentaires.

La suite de tests existe, dans `tests/`, mais elle repose sur `saddle`, un outillage ancien qui suppose une version de Node dépassée. La faire tourner aujourd'hui demande un travail que ce parcours n'a pas fait. Les scénarios de `spec/` restent lisibles et décrivent le comportement attendu.

Trois sujets sont volontairement laissés de côté. La **gouvernance** — le jeton COMP, le Governor et le Timelock de `Governance/` — mérite son propre parcours. L'**oracle de prix** n'est qu'une interface ici : la source réelle vit ailleurs, et c'est pourtant la dépendance la plus critique du protocole, puisque tout le chapitre 9 repose sur ses valeurs. Enfin, la **distribution du COMP** est mentionnée sans être détaillée.

Un point de vocabulaire pour éviter une confusion. Ce dépôt est Compound v2. **Compound III (Comet)** est une réécriture aux principes différents : un seul actif empruntable par déploiement, collatéraux qui ne rapportent pas d'intérêts. Rien de ce parcours ne s'y transpose directement.

Enfin, lire ces chapitres n'est pas auditer ce code. Comprendre un mécanisme n'est pas l'avoir vérifié.
