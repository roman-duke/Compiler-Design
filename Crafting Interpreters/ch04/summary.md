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


## Scanner
The core of the scanner is a loop. Now the follow up question is, what if this were being done in a functional programming language where there are no loops, would it still be easy to do with recursion? Starting at the first character of the source code, the scanner identifies a lexeme and then after consuming it, emits a token. It repeats the process for the next character in the source code.
The rules that determines how a language groups characters into lexemes are called its **lexical grammar**. In most programming languages, the rules of that grammar are simple enough for the language to be classified as a [**regular language**](https://en.wikipedia.org/wiki/Regular_language).


- ### Reserved Words and Identifiers
The principle of maximal munch states that when two lexical grammar rules can both match a chunk of code that the scanner is looking at, _whichever one matches the most character wins_.
For example, if we can both match "anderlecht" as an identifier and "and" as a keyword, then the former wins. In essence, you can't easily detect a reserved word until you've reached the end of what might instead be an identifier.
