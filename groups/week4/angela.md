# Report – Reverse Engineering, Hooks, Inheritance and Design Patterns in Pharo

## Introduction

This week, I started to explore reverse engineering in Pharo. 
I also took a closer look at hooks (`printOn:`, `asString`), inheritance, the difference between `self` and `super`, and some design patterns.
 Here’s how I approached these topics and what I learned.

## 1. Reverse Engineering in Pharo

Reverse engineering is all about analyzing an existing system to figure out how it works. In Pharo, that means:

- Browsing classes and their relationships in the class browser
- Reading method source code to understand logic
- Spotting design patterns
- Piecing together how everything fits

I found it helpful to just open up a class and start poking around, following method calls and seeing what each part does.

---

## 2. Hooks: `printOn:` and `asString`

Hooks are methods you’re supposed to override in subclasses to customize behavior. In Pharo, two common hooks are `printOn:` and `asString`.

- `printOn` is called when you print an object. You can override it to control how your object is displayed.
- `asString` returns a string representation of your object, usually by using `printOn:`.

**Example:**


Person >> printOn: aStream
   aStream nextPutAll: 'Person named ', name, ' (', age asString, ' years old)'.

Person >> asString
   ^ String streamContents: [ :s | self printOn: s ]


## 3. Inheritance, `self` and `super`

- **Inheritance** lets a class reuse code from its superclass. You can override methods to change or extend behavior.
- **`self`** sends a message to the current object (the instance).
- **`super`** sends a message to the superclass, skipping the current class’s implementation.

**Key difference:**
- `self` starts method lookup from the class of the instance.
- `super` starts lookup from the superclass.

**Example:**

Person >> description
   ^ 'A person named ', self name.

Student >> description
   ^ super description, ' (student)'.


## 4. Design Patterns in Pharo

Design patterns are reusable solutions to common design problems. In Pharo, you’ll see things like:

- **Singleton**: only one instance of a class
- **Observer**: objects get notified when something changes
- **Composite**: objects organized in a tree structure

You can spot patterns by looking at how classes interact and how responsibilities are divided.



That’s what I’ve been working on this week. Next, I’ll try to apply these ideas to a small system in Pharo.
