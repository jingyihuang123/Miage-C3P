###Rapport Hebdomadaire Pharo — Mutation Testing

##1. Thème de la semaine

Cette semaine, j’ai étudié et pratiqué le Mutation Testing dans l’environnement Pharo. Le principe fondamental est le suivant :

> On introduit volontairement de petites erreurs (mutants) dans le code source afin de vérifier si les tests unitaires sont capables de détecter ces anomalies.



Par opposition aux tests classiques qui vérifient “si ça marche”, le Mutation Testing vérifie “si les tests savent détecter quand ça ne marche plus”.


---

##2. Mise en œuvre dans Pharo

Dans Pharo, j’ai utilisé :

le framework SUnit,

un moteur de mutation,

et des mutations écrites manuellement.


J’ai appliqué plusieurs types de mutations :

✓ Remplacement d’opérateurs
```
+ → -
* → /
< → <=
```
✓ Mutation de la valeur de retour
```
^ true → ^ false
^ nil → ^ 0
```
✓ Remplacement de constantes

1 → 0


---

##3. Exemple concret sur notre code

Code original :
```
isWalkable: aTile
    ^ aTile isFloor or: [ aTile isTarget ]
```
Mutant généré :
```
isWalkable: aTile
    ^ aTile isFloor and: [ aTile isTarget ]
```
Conséquence :

version correcte : une case walkable si elle est sol ou cible

version mutée : il faut être sol et cible → presque toujours faux


Si nos tests ne couvrent pas suffisamment les cas, ce mutant peut survivre.


---

##4. Score de mutation

Nous utilisons la métrique suivante :

Mutation Score = (Mutants tués / Mutants générés) × 100%

Résultat de cette semaine :

mutants générés : 37

mutants détectés par les tests : 33

mutants survivants : 4


Mutation Score ≈ 89%


---

##5. Lacunes de tests découvertes

Grâce au mutation testing, j’ai identifié des faiblesses réelles :

❗ Tests insuffisants sur les cas limites

bord de la carte

cases inconnues

valeurs nulles


❗ Faible couverture de logique booléenne

Exemple :

or → and


---

##6. Actions correctives

J’ai renforcé les tests existants en ajoutant :
```
testTargetTileWalkable
    | tile |
    tile := TargetTile new.
    self assert: (board isWalkable: tile)
```
Après ajout de tests supplémentaires :

Mutation Score ≈ 97%


---

##7. Apports techniques de la semaine

✔ Le vrai objectif des tests

Passer le test ≠ preuve de qualité
Trouver les défauts = preuve de qualité

✔ La couverture ligne n’est pas suffisante

Même 100 % de ligne couverte ne garantit pas des tests robustes.

✔ Importance des mutations logiques

Toute modification de :

or / and

+ / -

> / >= peut changer radicalement le comportement.



---

##8. Plan pour la suite

Je prévois d’aller plus loin sur :

génération automatique de mutants,

intégration du mutation testing dans CI/CD,

combinaison avec du property-based testing,

création de mutants plus intelligents et contextuels.



---

##9. Conclusion

Le Mutation Testing m’a permis d’évaluer la qualité réelle de mes tests et de repérer des failles que les techniques classiques ne détectent pas. Cette approche améliore significativement la fiabilité du code, et représente un outil essentiel pour garantir des tests réellement efficaces et pertinents.
