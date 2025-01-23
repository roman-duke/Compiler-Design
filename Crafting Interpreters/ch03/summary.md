# Introduction
This chapter just introduces us to the Lox Programming language
in order to have a feel of the language we are trying to build an
interpreter for.

Lox shares the following features with languages like Javascript, Scheme and Lua.
- Dynamic Typing.
- Automatic garbage collection.

## Data Types in Lox
- Booleans
- Numbers
- Strings
- Nil. "null pointer errors have been dubbed the scourge of the industry"

## More notes
- The Scheme programming language has no built-in looping constructs, it relies on recursion for repetition. Smalltalk has no built-in branching construct, and relies on dynamic dispatch for selectively executing code.


### Classes
When it comes to making objects in OOP languages, there are actually two approaches to this - _classes and prototypes_. Classes were basically the only paradigm being talked about until "Javascript" accidentally took over the world.

#### Class-Based Languages
In these languages, there are two-core concepts: instances and classes. Instances store the state for each object and have a reference to the instance's class. Classes itself contains the methods and inheritance chain (hence why it is termed the blueprint for creating objects). To call a method on an instance, you simply look up the instance's class and you find the method there.


#### Prototype-Based Languages
In this paradigm, there are basically only objects and no classes.
