### Hook and Template

钩子与模板方法



### Introduction : Cours et vidéos  课程与视频说明

Cette semaine, dans le cadre du cours Hook and Template du module de Reverse Engineering,
j’ai étudié la manière dont Pharo met en œuvre le template method pattern (模板方法模式) et les hooks (钩子方法).
Ces mécanismes sont fondamentaux pour comprendre la réutilisation du comportement dans les hiérarchies de classes,
et ils sont très utiles en ingénierie inverse pour reconnaître les structures abstraites et les points d’extension du code.

本周在逆向工程模块的 Hook and Template（钩子与模板） 课程中，
我学习了 Pharo 如何实现 模板方法模式（template method pattern） 与 钩子方法（hook methods）。
这些机制是理解继承体系中行为复用的关键，
在逆向分析中，它们能帮助我们识别抽象结构与可扩展点，从而更好地理解代码框架的意图。

J’ai regardé les vidéos du MOOC correspondantes et expérimenté plusieurs exemples dans le Playground
afin de comprendre comment les sous-classes peuvent modifier ou compléter un comportement prédéfini par la superclasse.
我观看了相关的教学视频，并在 Playground 中复现了多个示例，
通过实验观察子类如何在不改变整体流程的前提下重写父类的部分逻辑。
---

###  Objectif / 学习目标

Comprendre le Template Method Pattern et la notion de Hook Method dans Pharo.

Identifier quelles parties du comportement sont fixées dans la superclasse et lesquelles sont ouvertes à la redéfinition.

Expérimenter par du code concret pour vérifier la réutilisation et la personnalisation du comportement.


理解 Pharo 中的模板方法与钩子方法机制；
区分父类中哪些逻辑是固定的、哪些是可被子类改写的；
通过代码实验验证行为的可复用性与可定制性。


## Exemple 1 – Template method / 模板方法示例
```
Meal >> prepare
    self cookMainDish.
    self serve.
    self cleanUp.

Meal >> serve
    ^ 'Serve on a plate.'

Meal >> cleanUp
    ^ 'Clean the kitchen.'

PastaMeal >> cookMainDish
    ^ 'Cook pasta with sauce.'

SoupMeal >> cookMainDish
    ^ 'Boil soup with vegetables.'
```
## Test / 测试：

(PastaMeal new) prepare.
(SoupMeal new) prepare.

Résultat attendu / 预期结果：

'Cook pasta with sauce. Serve on a plate. Clean the kitchen.'
'Boil soup with vegetables. Serve on a plate. Clean the kitchen.'

Analyse / 分析：
La méthode prepare agit comme un template :
elle fixe la structure générale de la préparation du repas,
tandis que cookMainDish est un point d’extension redéfini dans chaque sous-classe.

prepare 是模板方法，定义了整体流程，而 cookMainDish 是钩子点，
每个子类都可以在此基础上重写部分逻辑，实现不同的具体行为。



## Exemple 2 – Hook method / 钩子方法示例
```
Game >> play
    self initialize.
    self start.
    self playRound.
    self endGame.

Game >> playRound
    "Hook method"
    ^ self subclassResponsibility.

ChessGame >> playRound
    ^ 'Move pieces on board.'

PokerGame >> playRound
    ^ 'Deal and play cards.'
```
## Test / 测试：

(ChessGame new) play.
(PokerGame new) play.

Résultat attendu / 预期结果：

'Initialize game. Start. Move pieces on board. End game.'
'Initialize game. Start. Deal and play cards. End game.'

Observation / 观察：
La méthode playRound agit ici comme un hook :
elle est définie comme subclassResponsibility dans la superclasse,
ce qui oblige les sous-classes à fournir leur propre version.

playRound 是典型的钩子方法（hook method）。
父类声明了该方法但不实现，子类必须自行定义。
这使得整体流程可被复用，而细节可灵活变化。



### Réflexion personnelle / 个人反思

J’ai compris que le modèle Hook and Template permet d’éviter la duplication de code.
它通过在父类中定义固定结构、在子类中开放局部重写点，减少了代码重复。

En Reverse Engineering, reconnaître un template method aide à deviner
les intentions du concepteur et les zones d’extension du système.
在逆向分析中，识别出模板方法能帮助推测设计者的意图和可定制接口。

Les exemples fonctionnent comme prévu et m’ont permis de distinguer clairement
les comportements fixes et variables dans une hiérarchie d’héritage.
所有实验都符合预期，我清楚地分辨出继承体系中固定逻辑与可变逻辑的区别。

