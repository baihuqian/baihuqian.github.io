---
layout: post
title: Udacity Compiler Course
date: '2019-08-03 14:32'
published: true
---

This is the notes I took for [Compilers: Theory and Practice](https://classroom.udacity.com/courses/ud168), offered by Udacity.

* TOC
{:toc}

# Introduction to Compilers
Compiler is a program that *translates* a program from a *source language* into a *target language*. As a comparison, an interpreter is a computer program that directly executes instructions written in a programming or scripting language, without requiring them previously to have been compiled into a machine language program. An interpreter generally uses one of the following strategies for program execution:

1. Parse the source code and perform its behavior directly;
1. Translate source code into some efficient intermediate representation and immediately execute this;
1. Explicitly execute stored precompiled code[1] made by a compiler which is part of the interpreter system.

The goal of compilers is to avoid writing assembly program and increase developer productivity.

## How Compilers Work
A compiler consists of the front end and the back end. The **front end** of the compiler performs analysis:

1. Lexical analysis: through scanner;
2. Syntax analysis: through parser;
3. Semantic analysis: through semantic action.

The **back end** performs synthesis to convert the intermediate representation into assembly. This step is called code generation. All modern compilers perform optimizations, too.

**Scanner** goes through the entire file and produce tokens (equivalent to words in a book), which has a type and a value. For example, `157.669` is a token because it is surrounded by whitespaces, and its type is `FLOATCONST` (floating point constant) and the value is `157.669`. Besides constants, tokens include variables and keywords.

Scanning is the process of converting input text into stream of known objects called tokens. It simplifies the parsing process.

The **parser** requests tokens from the scanner and ensure that the returned token are legal in the "sentence". Parsing is the process of translating code to rules of grammar, building representation of code.

Once enough sentence is obtained, it verifies if the sentence has the right meaning. **Semantic Action** ensures the sentence that is being parsed currently is semantically meaningful. If the check fails, it returns a semantic error.

If the sentence is semantically meaningful, it is translated into an *intermediate representation* that is closer to the assembly. Scanner, parser, and semantic action together are called the **front end**, the analysis of program syntax and semantics and the transformation into the intermediate representation. Because the front end depends highly on the language syntax, it is very specific to a language.

Semantic analysis includes looking up variable name. Visible variable names are stored in the symbol table. If the sentence is a declaration of variables, it is put in the symbol table; if it is a use of variables, semantic checks are run to ensure the operation on the variable is valid.

*Lexical rules of language* dictate how legal word is formed by concatenating alphabet. Lexicon means dictionary.

*Grammar* dictates syntactic rules of language, i.e., how legal sentence could be formed.

## Scanning and Tokenization
Scanner takes a file as input and processes it character-by-character using a token buffer. Scanner reads characters into the token buffer until the next character no longer satisfies the lexical rules of the language, and a valid token is found. This process is called longest matching algorithm and it is used by all the scanners.

## Parser
Parser is the center of the front end, it controls the overall operation.

Parsing depends on the grammar rules. The following example illustrates grammar rules of a simplified `main` function of C:

```
<C-PROG> ➝ MAIN OPENPAR <PARAMS> CLOSEPAR <MAIN-BODY> // <C-PROG> is the start symbol. Every C program must start with a main function
<PARAMS> ➝ NULL // PARAMS can be NULL
<PARAMS> ➝ VAR <VARLIST> // PARAMS can be a single VAR, or a single VAR followed by a VARLIST
<VARLIST> ➝ , VAR <VARLIST> // VARLIST is comma-separated VARs followed by NULL
<VARLIST> ➝ NULL
<MAIN-BODY> ➝ CURLYOPEN <DECL-STMT> <ASSIGN-STMT> CURLYCLOSE
<DECL-STMT> ➝ <TYPE> VAR <VARLIST>;
<ASSIGN-STMT> ➝ VAR = <EXPR>;
<EXPR> ➝ VAR
<EXPR> ➝ VAR <OP> <EXPR>
<OP> ➝ +
<OP> ➝ -
<TYPE> ➝ INT
<TYPE> ➝ FLOAT
```

Note that the grammar rule does not specify everything about a language, e.g. type restrictions, and it is up to semantic checks. The reason is that parsing is context *insensitive* but semantic analysis is context *sensitive*. It is more expensive to perform semantic analysis than parsing.

Parser works by matching tokens with rules. When match takes place at certain position, move further (get next token & repeat). If expansion needs to be done, choose appropriate rule. If no rule found, declare error. If several rules found, the grammar (set of rules) is ambiguous, and well-designed grammar rules should not be ambiguous.

Expansion determines how parts in `<>` get interpreted as they have multiple ways of matches.

### Ambiguity
Ambiguity can lead to understanding a given sentence in two different ways, which allows assigning two meanings to the same sentence, something highly undesirable. It is up for the language designers to remove ambiguity in the language syntax.

## Semantic Analysis
### Symbol Table
The symbol table shows us what are the different values which are declared and available in the current scope. Most of languages are block-based, which means declaration available in the current scope is also available in inner scopes unless they are redeclared.

A basic symbol table contains the name and the type of variables as well as the scope in which they are available. Semantic analysis then look up symbols to see if they are declared and to determine the types of them.

### Semantic Actions
There are the many types of semantic actions. Some examples include:
1. Enter variable declaration into the symbol table;
2. Look up variables in the symbol table;
3. Do binding analysis of looked-up variables based on scoping rules. From the innermost scope upward, find the variable declaration and bind the variable to it.
4. Type checking for compatibility;
5. Keep the semantic context of processing. When a subexpression is checked, the subexpression itself has a type. This process keeps track of the type for further checks that the subexpression participates. For example, expression `a + b + c` can be broken down into subexpression `t1 = a + b` and `t2 = t1 + c`. The process keeps track of the type of `t1` and `t2` when they are processed.

They can be largely grouped into two categories:
1. Checking: binding, type compatibility, scoping, etc.
2. Translation: generate temporary values, propagate them to keep semantic context.

A simple way of implementing semantic analysis is to embed action symbols in the grammar. An *action symbol* is to tell us about a *semantic procedure* which is being triggered at that particular point during parsing. Semantic procedures contains semantic actions and return values. Semantic procedures are called by the parser at appropriate places during parsing. *Semantic stack* can be used to implement and store semantic records.

Semantic actions can be embedded in the grammar like this:

```
<decl-stmt> ➝ <type>#put-type<varlist>#do-decl
<type> ➝ int | float
<varlist> ➝ <var>#add-decl <varlist>
<varlist> ➝ <var>#add-decl
<var> ➝ ID#proc-decl
#put-type puts given type on semantic stack
#proc-decl builds decl record for var on stack
#add-decl builds decl-chain
#do-decl traverses chain on semantic stack using backwards pointers entering each var into symbol table
```

Declaration chain means all related variables are of the same type defined in `put-type`.

# RegEx Det. Finite Automata
