Week5 huangjingyi


Cette semaine, j’ai regardé les vidéos recommandées dans le MOOC 



Ces vidéos m’ont aidé à mieux comprendre les mécanismes fondamentaux de Pharo.
Je les ai regardées tout en testant les exemples de code dans le Playground afin de vérifier si leur comportement correspondait bien aux explications données dans les vidéos.
这些视频帮助我更好地理解了 Pharo 的核心机制。
我边看边在 Playground 中尝试了示例代码，以验证它们的行为是否与视频中解释一致。





Réviser le comportement des méthodes de classe et de l’instruction super en Pharo à travers plusieurs petits exemples pratiques.
通过多个实际示例复习 Pharo 中 类方法（class methods） 与 super 指令的工作机制。


 1. Comprendre super / 理解 super
Animal >> speak
    ^ 'Animal sound'

Dog >> speak
    ^ super speak , ' Woof!'

Cat >> speak
    ^ super speak , ' Meow!'

Explication :
解释：
    • super ne désigne pas l’objet lui-même, mais la superclasse de la classe actuelle。
    • Cela permet d’appeler la version héritée d’une méthode, tout en y ajoutant un comportement spécifique.
super 并不指当前对象本身，而是指当前类的父类。
它允许我们在调用父类方法的同时，添加子类特有的行为。


Résultat attendu 预期输出：
(Dog new) speak.   →  'Animal sound Woof!'
(Cat new) speak.   →  'Animal sound Meow!'
Vérification / 检查是否按预期工作：
Oui, les deux méthodes appellent d’abord Animal >> speak, puis ajoutent leur propre son.
是的，两种动物都先调用了父类的 speak，然后再追加自己的发声，结果符合预期。



 2. Tester un comportement inattendu / 检验非预期行为
Dog >> speak
    ^ super speak.
Si la classe Animal ne définit pas de méthode speak,
Pharo renverra une erreur :
MessageNotUnderstood: Animal>>speak
Analyse / 分析：
Cette expérience montre que super ne vérifie pas automatiquement l’existence d’une méthode.
Il faut donc connaître la hiérarchie avant d’utiliser super.
这说明 super 并不会自动检查方法是否存在，使用前必须了解继承层次结构。

3. Méthodes de classe / 类方法
Animal class >> kingdom
    ^ 'Animalia'

Dog class >> species
    ^ 'Canis lupus familiaris'

Cat class >> species
    ^ 'Felis catus'
Résultat 输出：
Animal kingdom.  → 'Animalia'
Dog species.     → 'Canis lupus familiaris'
Cat species.     → 'Felis catus'
Explication 解释：
Les méthodes de classe sont appelées sur la classe elle-même, et non sur une instance.
它们属于类，而不是对象，用于描述整个类别的信息。
Par exemple, Dog species donne des informations générales sur le type de chien,
tandis que (Dog new) speak décrit le comportement d’un individu.
例如，Dog species 提供物种信息，而 (Dog new) speak 是单个实例的行为。

4. Interaction entre classe et instance / 类与实例的交互
Counter class >> newWithValue: n
    | c |
    c := self new.
    c value: n.
    ^ c
Explication / 解释：
Cette méthode de classe crée un nouvel objet (instance method) avec une valeur initiale.
类方法可以用来封装实例化过程，直接返回带初始值的对象。
Test / 测试：
(Counter newWithValue: 5) value.  →  5
Résultat attendu / 预期：
Fonctionne comme prévu, prouvant que les méthodes de classe peuvent collaborer avec les méthodes d’instance.
结果符合预期，

5. Comment mieux comprendre leur fonctionnement ? / 如何更好理解它们？
    • J’ai utilisé le Playground pour exécuter chaque exemple et comparer les sorties.
我在 Playground 中执行每个例子，对比输出结果。
    • J’ai ouvert le Class Browser pour observer la chaîne d’héritage (Method Lookup Chain).
我通过类浏览器查看了方法查找链，理解 super 是如何定位到父类方法的。
    • J’ai aussi testé des combinaisons inhabituelles (super dans une méthode de classe) pour comprendre leurs limites.
我尝试在类方法中使用 super，发现这会导致错误，


Les exemples fonctionnent comme prévu et m’ont permis de bien distinguer :
      Les méthodes d’instance (agissent sur un objet)
      Les méthodes de classe (agissent sur la classe elle-même)
      Le rôle précis de super dans la hiérarchie d’héritage
这些实验都按预期工作，让我清楚区分了：
      实例方法：作用于对象；
      类方法：作用于类本身；
      super：在继承层级中调用父类方法的机制。
