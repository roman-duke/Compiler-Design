# Paths of a Language Implementation
The network of paths of a programming language implementation can be broadly classified into the following:

+ ### Scanning:
This is also termed **lexing** (or **lexical analysis**). A lexer takes in a stream of characters and chunks them into something more akin to **words**. These words can be termed a **tokens**. Whitespaces and comments are ignored by the language.

+ ### Parsing:
This is where our syntax gets a grammar. The ability to compose larger expressions and statements out of smaller parts. A parser builds a tree structure from the flat sequence of tokens. This tree can be termed a **parse tree** or **abstract syntax tree**. In computer science, parsing has a long rich history that is tied to the artificial intelligence community.

+ ### Static Analysis:
This is where particular language implementations start to differ. At this point, we know the syntactic structure of the code, but we are still quite limited. For example:
```a + b```. We know we are dealing with two variables (a and b), but we don't know if they are local or global variables or if they are defined. The first bit of analysis that most languages do is termed **binding** or **resolution**.

+ ### Intermediate Representations:
The frontend of a language implementation is specific to the language the program is written in, while the backend is concerned with the final architecture where the program will run. The intermediate representation acts as an interface between the source and destination. It reduces the effort of writing writing compilers for mulitple source languages and target platforms. Imagine you want to implement Pascal, COBOL and Fortran compilers for the target environments in x86, ARM and SPARC. This would imply 9 full compilers in total. The difference becomes more apparent when the number of languages and environments scale up.

+ ### Optimization:
After we understand what the user's program means, we can then choose to swap it out with a different program that has the same semantics but implements them more efficiently. An example of this would be **constant folding**: if some expression always evaluates to a particular value, then we can do the evaluation at compile time and replace the code for the expression with its result.

+ ### Code Generation:
After applying all the optimizations, the last step is converting it to a form that the machine can actually run. We could convert it directly to a primitive assembly-like instructions for a particular CPU or we could convert it to a form for a hypothetical idealized machine. The first approach is lightning fast, but not portable, the second approach isn't as fast but is very portable.

+ ### Virtual Machine:
A program that emulates a hypothetical chip supporting your virtual architecture at runtime.

+ ### Runtime:
While the code is running, it usually needs some sort of services that the language provides. For example, a "garbage collector". All of this lives inside a **runtime**.
