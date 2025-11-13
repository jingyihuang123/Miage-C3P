#Rapport d’apprentissage — Semaine6 (Design avancé / Pharo)

##1. Le motif Composite

Cette semaine, j’ai commencé par réviser le motif Composite, un modèle que j’ai déjà vu mais dont l’utilité devient beaucoup plus claire maintenant.

L’idée principale :

> Permettre de traiter un objet simple et un objet composé exactement de la même façon.



Ce que j’ai compris

On définit une interface commune (Component).

Les Leaf (objets simples) et les Composite (objets contenant d’autres objets) répondent aux mêmes messages.

Le client n’a jamais besoin de distinguer “c’est un noeud ? un leaf ?”.


Pourquoi c’est utile ?

Évite les if class = Leaf ... ou les cas particuliers.

Très adapté pour les structures hiérarchiques :
menus, widgets graphiques, fichiers/dossiers, scènes graphiques.



---

## L’héritage et le polymorphisme (rappel appliqué dans les slides)

Même si ce n’était pas un module séparé, toute la semaine repose sur un point fondamental :

Le polymorphisme = éviter les tests conditionnels

Donc :

Au lieu de tester des cas dans une méthode,
→ on crée plusieurs petits objets qui répondent chacun différemment au même message.

Le comportement change par dispatch dynamique, pas par if.


C’est la base du Composite, mais aussi du NullObject et du Fluid API.


---

## Pourquoi il faut éviter de renvoyer nil

C’est un des messages les plus importants du cours.

> Un nil renvoyé par un fournisseur oblige tous les clients à faire ifNil: ou ifNotNil:.



Problèmes :

Pollution du code client.

Risque d’oublier un test.

Propagation du nil dans tout le système.


Le cours explique qu’un bon objet doit, autant que possible :

✔ toujours retourner un objet utilisable

✔ toujours être correctement initialisé
✔ ne jamais forcer ses clients à vérifier des cas spéciaux


---

## Alternatives : retourner des objets polymorphes

Quand un résultat peut être vide :

Situation Mauvais réflexe Bon réflexe

Pas d’éléments nil #() (collection vide)
Pas de valeur nil 0
Rien à faire nil objet “ne fait rien”


Ça simplifie énormément le code client.


---

## Le motif NullObject

C’est l'exemple le plus parlant du cours.

Au lieu de gérer :
```
selectedTool ifNotNil: [:t | t attachHandles ].
```
On initialise toujours l’attribut avec un objet spécial :
```
selectedTool := NoTool new.
```
NoTool a les mêmes méthodes que les vrais outils, mais ne fait rien :
```
NoTool >> attachHandles
  ^ self
```
Avantages retenus

Le client n’a plus jamais besoin de vérifier ifNil:.

Le code devient plus fluide et plus lisible.

Le “non-comportement” est encapsulé dans un objet réutilisable.


Inconvénients mentionnés

Parfois difficile de définir le “comportement par défaut”.

Peut demander beaucoup de NullObjects différents selon l’API.



---

## Importance de l’initialisation (initialize)

Pharo initialise toutes les variables d’instance à nil.
Le cours insiste sur cette responsabilité :

✔ C’est au développeur d’initialiser chaque variable correctement.

Exemple :
```
initialize
  super initialize.
  members := OrderedCollection new.
```
Objectif :
éviter que les attributs soient nil → éviter propagation → éviter tests.


---

## Reification : transformer un comportement en objet

C’est la philosophie derrière beaucoup d’exemples :

ClassHierarchyPrinter

DatePrinter

ZTimestampFormat

etc.


Au lieu d’écrire une méthode avec 10 paramètres, on crée : → un petit objet configuré, avec ses propres attributs.

Avantages compris

API plus simple

évite explosion des paramètres

plus modulaire

plus facile à tester

plus facile à étendre


Ça montre la différence entre “faire une méthode” et “faire un objet qui sait faire quelque chose”.


---

## Fluid API

C’est un style d’API basé sur la cascade ;, pour éviter les méthodes avec trop de paramètres.

Exemple classique :
```
html heading
  level: 3;
  with: 'Hello'.
```
Points importants que j’ai retenus

Pros

Évite l’explosion combinatoire des paramètres.

Gère naturellement les arguments optionnels.

Encourage l’initialisation avec de bons defaults.

Lecture plus naturelle.


Cons

L’ordre des messages peut parfois compter.

Nécessite toujours une “closing message” (with:).

Le builder doit exister.


C’est surtout utilisé dans Seaside et dans certaines APIs du système (transcript, builders).


---

## Builder API

Le cours explique aussi un autre rôle structurel :

message créateur (builder)

messages de configuration

message de fermeture


Dans Pharo, la syntaxe
```
Object subclass: #Point
    instanceVariableNames: 'x y'
    package: 'Kernel'
```
est en réalité un Builder API.

Cela permet d’avoir :

une syntaxe compacte

modulaire

extensible

évite la “méthode géante avec 15 arguments”
