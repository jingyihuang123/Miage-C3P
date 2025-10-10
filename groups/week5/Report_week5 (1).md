# Week 5 Report 

**Vonjiniaina Angela Béatrice Fanomezantsoa** 
**Module:** Reverse Engineering – Week 4  
**Topic:** Classes, Messages, `super`, and Double Dispatch

---

## What I Learned This Week

This week I spent time learning how object-oriented programming works in Pharo so far. Since it’s a new language for me, I’m still getting used to the syntax and how things are structured. I focused on creating classes, sending messages between objects, and using inheritance with `super`.

I also explored the concept of double dispatch, which was a bit confusing at first, but I think I’m starting to understand it better. To make things easier, I used animals as examples.

---

## Class and Method Examples

I started by creating a general `Animal` class with basic properties like `name` and `description`. Then I created a `Cat` subclass that adds a behavior: `meow`.

### Animal Class

```
Object << #Animal
   slots: {#name. #description};
   package: 'ZooApp'.

Animal >> initialize
   name := 'Unknown'.
   description := 'No description yet'.

Animal >> name: aName
   name := aName.

Animal >> description: aDescription
   description := aDescription.

Animal >> introduce
   Transcript show: 'Hi, I am ', name, '. ', description.
```

### Cat Subclass

```
Animal << #Cat
   slots: {};
   package: 'ZooApp'.

Cat >> initialize
   super initialize.

Cat >> meow
   Transcript show: 'Meow! My name is ', name.
```

### Playground Test

```
Transcript open.

myCat := Cat new.
myCat name: 'Luna'.
myCat description: 'I like sleeping on keyboards.'.

myCat introduce.
myCat meow.
```

**Expected Output:**
```
Hi, I am Luna. I like sleeping on keyboards.
Meow! My name is Luna
```

---

## Double Dispatch Example – Meowing

Double dispatch allows two objects to decide together what behavior should happen. Instead of using a bunch of `if` statements, each object knows how to respond based on the other.

Here’s an example where animals meow at each other:

```
Animal >> meowAt: anotherAnimal
   ^ anotherAnimal respondToMeowFrom: self
```

Each subclass defines how it reacts to a meow:

```
Cat >> respondToMeowFrom: anAnimal
   Transcript show: 'Cat meows back at ', anAnimal class name.
```

```
Dog >> respondToMeowFrom: anAnimal
   Transcript show: 'Dog barks at ', anAnimal class name.
```

### Playground Test

```
luna := Cat new.
buddy := Dog new.

luna meowAt: buddy.
buddy meowAt: luna.
```

**Expected Output:**
```
Dog barks at Cat
Cat meows back at Dog
```

---

## Summary

This week was a mix of confusion and progress. I’m slowly getting used to Pharo and how object-oriented programming works. Using animals helped me visualize the concepts better. I now understand how to create classes, send messages, use `super`, and apply double dispatch to make objects interact in more dynamic ways.

Looking forward to learning more next week.
