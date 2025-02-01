# A Tree-Walk Interpreter
This is the beginning of the implementation of jlox, the first of our two interpreters.

## Scanning
This chapter 4 implements the first step of compilation - scanning. A scanner takes in raw source code as a series of characters and groups it into a series of chunks termed **tokens**. Before, the term "scanning" was used to refer to the phase where the source code characters are read from disk and buffered into memory. Then "Lexing" used to refer to the subsequent phase of doing useful stuff with the characters. But now that first step is very trivial that scanning and lexing have been merged together to mean the same thing.


## Lexemes
Take a look at the following line of code;
`var language = 'lox';`

Lexical anaylysis entails parsing the sequence of characters and identifying that smallest sequences that still represent something. Each of the blobs that still represents something is termed a "lexeme". Put more formally:

> A lexeme is a sequence of characters in the source code that matches the pattern for a token and is identified by the lexical analyzer as an instance of that token.

> A token is a pair consisting of a token name and an optional attribute value. The token name is an abstract symbol representing a kind of lexical unit.

Also, [read this](https://stackoverflow.com/questions/14954721/what-is-the-difference-between-a-token-and-a-lexeme) for a more beautiful explanation.


## Token types
A parser could categorize tokens from the raw lexemes by comparing the srtrings, but it's not the most efficient data structure. We could use a data structure where upon encountering a lexeme, we remember what kind of lexeme it represents.
