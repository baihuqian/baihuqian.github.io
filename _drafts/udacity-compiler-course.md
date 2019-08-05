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
<C-PROG> ➝ MAIN OPENPAR <PARAMS> CLOSEPAR <MAIN-BODY> // Every C program must start with a main function
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