# Homework / Report — Kata Practice  



##  What I did this week

This week, our class focused on **Kata exercises** in Pharo to strengthen our understanding of **object-oriented programming** principles such as encapsulation, inheritance, and polymorphism.

*本周我们主要进行了 Pharo 的 Kata 编程练习，以强化对面向对象编程（OOP）核心概念的理解，包括封装、继承与多态。*

I practiced several small Katas where I had to design and refactor objects step by step. These included:
- Creating classes with clear responsibilities.  
- Implementing methods and improving cohesion.  
- Applying inheritance where appropriate.  
- Using tools like **Playground**, **Debugger**, and **Inspector** to test and analyze behavior.

*我通过多个小型 Kata 练习分步设计与重构对象，包括：创建职责清晰的类、实现方法并提升内聚性、合理使用继承、并借助 Playground、Debugger 与 Inspector 等工具测试代码行为。*

This helped me gain a deeper intuition for how messages are sent between objects and how dynamic dispatch determines which method is executed at runtime.

*这些练习让我更直观地理解了对象之间的消息发送机制，以及动态分派在运行时如何决定实际调用的方法。*



##  Reflection on the Kata practice / 对 Kata 练习的反思

- **Main difficulty:**  
  At first, I tended to write procedural code instead of object-oriented structures. The pratices forced me to think more in terms of “responsibility per object”.

  *一开始我容易写出过程化的代码，而练习让我重新思考“每个对象应承担什么职责”。*

- **What I learned:**  
  Rewriting code step by step improved readability and structure. I also learned to use **tests** to confirm that the system still behaves correctly after modification.

  *通过逐步重写，我学会了如何提升代码可读性和结构；同时学会使用测试验证系统行为未受破坏。*

- **Pharo tools used:**  
  - **Playground** for experimentation  
  - **Debugger** to inspect live object state  
  - **Senders/Implementors** to trace message calls  
  - **Inspector** to visualize internal variables





##  Small example 示例

Below is a simplified version I worked on: **Counter**

```smalltalk
Object subclass: #Counter
    instanceVariableNames: 'value'
    package: 'KataPractice'.

Counter >> initialize
    value := 0.

Counter >> increment
    value := value + 1.

Counter >> decrement
    value := value - 1.

Counter >> value
    ^ value.

| c |
c := Counter new.
c increment; increment; decrement.
Transcript show: c value. "=> 1"

What I learned:
Encapsulation helps prevent direct variable access; only messages should control internal state.
我学到封装的重要性：不要直接访问变量，而是通过消息操作内部状态。
```
 
## Conclusion / 总结

The Kata sessions were highly beneficial for consolidating object-oriented thinking.
I improved my understanding of message sending, object responsibilities, and refactoring discipline.



