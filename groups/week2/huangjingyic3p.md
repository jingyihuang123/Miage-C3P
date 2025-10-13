Homework 

(Module 2 – OOP / Pharo)

### 1) Ce que j’ai fait cette semaine

观看了老师推荐的 Pharo MOOC 视频，并边看边在 Playground 里复现示例，检查行为是否与讲解一致。
J’ai visionné les vidéos du MOOC Pharo et j’ai reproduit les exemples dans le Playground pour vérifier leur comportement.

练习并整理了 message dispatch（消息分派） 的若干小实验：self / super、重写与动态绑定、类方法与实例方法、以及简单的 double dispatch 模式。
J’ai conçu plusieurs mini-expériences sur le dispatch : self/super, override + liaison dynamique, méthodes de classe vs d’instance, et un petit exemple de double dispatch.

用到的工具：Senders/Implementors、Debugger/Inspector、Transcript，以及 Help Browser / Pharo by Example 查询规则。
Outils utilisés : Senders/Implementors, Debugger/Inspector, Transcript, et Help Browser / Pharo by Example.


## 2) Practice message dispatch · 实践与代码

> 要求：写小例子挑战自己对 dispatch 的理解；代码是否如预期；不同点在哪里；如何修正认知并找到信息。
Exigence : écrire de petits exemples, vérifier l’écart entre attentes et réalité, corriger les hypothèses et documenter la démarche.



## 2.1 self 与 super 的分派差异

Object model：self 按运行时接收者动态查找；super 以当前类的超类为起点，但接收者仍是同一个对象。

Object subclass: #Animal
    instanceVariableNames: ''
    classVariableNames: ''
    package: 'DispatchLab'.

Animal >> sound
    ^ 'Animal'

Object subclass: #Dog
    instanceVariableNames: ''
    classVariableNames: ''
    package: 'DispatchLab'.

Dog >> sound
    ^ super sound , ' + Dog' "起点在 Animal，但接收者还是 Dog 实例"

Dog >> speak
    ^ self sound "动态绑定到 Dog >> sound（这里会走上面的方法）"

预期：Dog new speak 应显示 'Animal + Dog'。
实际：与预期一致。super 并不是“换了接收者”，只是改变查找起点。
修正来源：在 Debugger 里单步、看 self class 未变；查 Pharo by Example 的 message lookup 章节。



## 2.2 动态绑定 vs 变量静态名义类型

| a |
a := Animal new.
a := Dog new. "同一变量，运行时绑定到 Dog"
a sound. "=> 'Animal + Dog'（经上例覆盖/链式实现）"

结论：Smalltalk 始终动态分派；变量的“声明类型”不起限制作用。
Conclusion : liaison dynamique pure ; le type nominal n’impose pas le dispatch.

## 2.3 类方法（metaclass 侧）与 super

Animal class >> who
    ^ 'Animal class'

Dog class >> who
    ^ super who , ' -> Dog class'

观察：类方法在 metaclass 链 上查找；super 起点是 Dog class 的超类（Animal class）。
收获：理解“类也是对象，类方法走 metaclass 继承树”。
Appris : les méthodes de classe suivent la hiérarchie des métaclasses.


## 2.4 初始化链与 super initialize 的影响

Animal >> initialize
    Transcript show: 'A-init; '; cr.

Dog >> initialize
    super initialize.
    Transcript show: 'D-init; '; cr.

Dog new 时 Transcript 输出顺序：A-init; 然后 D-init;。
反思：忘记 super initialize 会漏掉父类必需的初始化。
Sans super initialize, on perd l’initialisation de la superclasse.


## 2.5 迷你 Double Dispatch（绘制示例）

Object subclass: #Block
    instanceVariableNames: ''
    package: 'DispatchLab'.

Block >> drawOn: aView
    ^ aView drawBlock: self.

Object subclass: #WallBlock
    instanceVariableNames: ''
    package: 'DispatchLab'.

WallBlock >> drawOn: aView
    ^ aView drawWall: self.

Object subclass: #MyView
    instanceVariableNames: ''
    package: 'DispatchLab'.

MyView >> drawBlock: aBlock
    ^ 'draw generic block'.

MyView >> drawWall: aWall
    ^ 'draw WALL block'.

| v b w |
v := MyView new.
b := Block new. w := WallBlock new.
{ b drawOn: v. . w drawOn: v } "=> #('draw generic block' 'draw WALL block')"

要点：第一次分派由方块 drawOn: 选择消息名称；第二次分派由 MyView 根据具体子类执行不同实现。
Le premier envoi choisit le nom du message, le second exploite le type concret sur la vue.




大多符合；两个易错点：① 误以为 super 改变接收者；② 忘记在子类里调用 super initialize。
La plupart ont confirmé mes attentes ; deux pièges : croire que super change le receveur ; oublier super initialize.

预期与现实有什么不同？
类方法的继承发生在 metaclass 链上，这点起初不直观。
La hiérarchie des métaclasses pour les méthodes de classe était moins intuitive.

如何修正假设？如何找到信息？
用 Debugger 观察 self class 不变；查 Senders/Implementors 看真正被调用的方法；阅读 Pharo by Example 与 Help Browser 的 Message Lookup 条目，配合 Transcript 打印确认执行顺序。
J’ai utilisé le Debugger, Senders/Implementors, Transcript et la doc (Pharo by Example, Help Browser) pour valider et corriger mes hypothèses.




### Conclusion

通过这些最小实验，我更加清楚：Smalltalk 的分派是严格基于运行时接收者的动态分派；super 只是改变查找起点而不是改变接收者；类方法沿 metaclass 继承；而 double dispatch 可优雅地把“对象种类 × 视图/操作种类”的组合复杂度拆解开来。
Ces expériences confirment la liaison dynamique, le rôle de super, la chaîne des métaclasses et l’intérêt du double dispatch pour gérer les combinaisons de types
