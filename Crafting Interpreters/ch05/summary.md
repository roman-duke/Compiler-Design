# Representing Code

> To dwellers in a wood, almost every species of tree has its voice as well as its feature ~ Thomas Hardy, Under the Greenwood Tree.

## Introduction
The previous chapter walked us through the process of transforming the source code of strings into a sequence of tokens.
The job of the parser is to take those tokens and transform them into a richer and more complex representation. But before that happens,
we need a way to represent the code so that it'd be much easier for the parser. Now the question is, how exactly do we go about doing this??
Have a look at the following expression below:

`2 + 5 * 3 - 1`

By looking at that expression and using the good old rule of BODMAS, we can easily arrive at the right answer (using the precendence in the rule).

A good way to visualize the precedence is using a tree, leaf nodes are numbers and root nodes are the operators which branches for each of their operands.
In order to resolve this tree, we need to start from the leaves up to the root (we need to know the values of the subtrees). This is called a _post-order traversal_.
The following ASCII diagrams show the process of evaluation of this tree (the bold leaf indicates that it's newly been evaluated from the previous process):

A.               -
                / \
               +   1
              / \
             2   *
                / \
               5   3


B.               -
                / \
               +   1
              / \
             2 **15**


C.               -
                / \
          **17**   1


D.             **16**


Now the point to note is that no matter how complex an arithmetic expression is, once given a tree like this evaluation becomes extremely easy.

Following this logic, we can intuitively guess that a working representation of our code is a tree structure that matches the grammatical strucutre
of the language. Hence we need to get more formal about our definition of grammar as categorized and defined by the [Chomsky hierarchy](https://en.wikipedia.org/wiki/Chomsky_hierarchy).


## Context-Free Grammars
In the last chapter, the rules we used for defining the lexical grammar were those provided by a refular language.
Since all our scanner does is emit sequence of tokens, that was fine. But regular languages can't get you very far as they are not capable of handling nested expressions.

The next tool in the toolbox of _[formal grammars](https://en.wikipedia.org/wiki/Formal_grammar)_ that builds upon regular language is a **context-free grammar (CFG)**.
By definition, a formal grammar takes a set of atomic pieces (let's call it alphabet) and then defines a set of "strings" that are in the "grammar" (usually infinite).

### Rules for grammars
Now the main question is, how do we write a grammar that creates an infinite set of strings? We do this by having a set of rules for generating the strings. Each rule
in a context-free grammar has a **head** (its name) and a **body** (which describes what it generates - a list of symbols).
The symbols can either be one of the following:
- A **terminal** is a letter from the grammar's alphabet.
- A **non terminal** is a named reference to another rule in the grammar.

Now, we have to think of a way to write down these rules. There's a notation termed the **[Backus-Naur form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form)**
which tends to influence the notation most people use.

Now, let's have a rule for football players. Each rule is a name, followed by an arrow (→) followed by a sequence of symbols, and then ending with
a semi-colon.

```
player -> winger "and" player "who are" relationship "are" relationship;
player -> name;
player -> defender;

winger -> attribute name;

defender -> "Virgil Van Dijk";
defender -> "Nemanja Vidic";
defender -> "Sergio Ramos";

attribute -> "skillful";
attribute -> "strong";
attribute -> "fast";

name -> "Ronaldo";
name -> "Neymar";
name -> "Messi";
name -> "Leroy Sane";
name -> "Marcus Rashford";

relationship -> "rivals";
relationship -> "teammates";
relationship -> "enemies";
relationship -> "friends";
```

Let's come up with a sample example by picking the first rule of the grammar:
- The first non-terminal symbol there refers to the rule (_`attribute name`_).
  So we can substitute that with say, "skillful Neymar".
- Next, we substitute player with another of its rules. Say we pick just name
  this time around. "Sergio Ramos".
- Next, we substitute the next non-terminal symbol relationship with say "rivals".
- Lastly, we substitute the last non-termainal symbol with "friends".
- This gives us our final expression (our generated string) as:
  - `skillful Neymar and Sergio Ramos who are rivals are friends`.

The above example makes sense for the seemingly contrived choices picked for the symbols.
But what if we had a case like "fast Leroy Sane and Marcus Rashford who are rivals are rivals".
Based on the syntax of the grammar that is perfectly correct, but does that sound correct? What
does it mean for it to even sound correct. Does that have to do with the semantics? If so, where is
that defined?

### Enhancing our notation
This is essentially just a more compact way to express our notation. We can rewrite our rules above
above as the following:

```
player -> winger "and" player "who are" relationship "are" relationship | name | defender;

winger -> attribute name;

defender -> "Virgil Van Dijk" | "Nemanja Vidic" | "Sergio Ramos";

attribute -> "skillful" | "strong" | "fast";

name -> "Ronaldo" | "Messi" | "Leroy Sane" | "Marcus Rashford";

relationship -> "teammates" | "enemies" | "friends";
```

## A Grammar for Lox Expressions
Now back to the actual lox language. Due to how large the syntatical grammar is, we have to use a subset
of our language in the next couple of chapters and then add more features on top of it later. These are the
expressions to worry about:
- Literals: Numbers, strings, booleans and nil.
- Unary expressions: ! and -.
- Binary expressions: The infix arithmetic and logic operators.
- Parentheses: '(' and ').

With the new notation we have, our grammar for those listed above is given as:

```
expression  -> literal
              | unary
              | binary
              | grouping;

literal     -> NUMBER | STRING | "true" | "false" | "nil";

grouping    -> "(" expression ")";

unary       -> expression operator expression;

operator    -> "==" | "!=" | "<" | "<=" | ">" | ">="
              | "+" | "-" | "*" | "/";
```

## Implementing Syntax Trees
Hmmmm, it is stated that since grammar of the language is recursive, then then data structure that represents
the grammar will form a tree. That is an interesting point to look at. [Read here](https://stackoverflow.com/questions/48592044/can-every-recursion-be-converted-to-trees).


### Metaprogramming the trees
We have a bunch of behaviour-less classes to model the type of each rule. Because there are going to
be a lot of time, we can simply meta-program it because they all have similarities (each contains
a name and a list of typed fields).
