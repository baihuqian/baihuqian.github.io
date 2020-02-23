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
1. Explicitly execute stored precompiled code made by a compiler which is part of the interpreter system.

The goal of compilers is to avoid writing assembly program and increase developer productivity.

## How Compilers Work
A compiler consists of the front end and the back end. The **front end** of the compiler performs analysis:

1. Lexical analysis: through scanner;
2. Syntax analysis: through parser;
3. Semantic analysis: through semantic action.

The **back end** performs synthesis to convert the intermediate representation into assembly. This step is called code generation. All modern compilers perform optimizations, too.

**Scanner** goes through the entire file and produce tokens (equivalent to words in a book), which has a *type* and a *value*. For example, `157.669` is a token because it is surrounded by whitespaces, and its type is `FLOATCONST` (floating point constant) and the value is `157.669`. Besides constants, tokens include variables and keywords.

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

# Lexical Analysis
Scanner performs lexical analysis, which reads input one character a time, group characters into tokens, remove whitespaces and comments, and encode token types and form tuples of `<token-type, value>` and return to parser.

Lexical analysis is based on lexical rules, which determines how to form legal strings. They are expressed using regular expression `r` and its language `L(r)`, and analyzed with finite automata, a type of state machine.

## Regular Expression
A symbol is a valid character in a language, and alphabet, $$\Sigma$$, is the set of legal symbols.

Regular expression contains metacharacters/metasymbols that have special meanings:
* defining reg-ex operations (e.g. `|`, `(`, `)`, `*`, `+`, etc.)
* Escape character `\` to turn off special meanings
* Empty string $$\epsilon$$ and empty set $$\phi=\{\}$$

Basic regular expression operations include
* Alternation, denoted by `a | b`,
* Concatenation， denoted by `ab`,
* Repetition (Kleen closure, 0 or more times) `a*`.

The precedence is repetition > concatenation > alternation. Parentheses can be used to change precedence.

Unix-style regular expressions introduces shorthands to simplify regular expressions.
* `.` denotes any character in the alphabet
* `[]` denotes a character class, allowing range (e.g. `[a-d]`) and complement (e.g. `[^1-3]`)
* `+` repeat one or more times
* `?` optional (zero or one time)
* `^` and `$` denote beginning and end of line

We can use regular expression to specify valid tokens. For example,
* `[0-9][0-9]*` is unsigned integer, and `(+|-)?[0-9][0-9]*` is signed integer.
* `[+|-]?[0-9]+(\.[0-9]*)?([eE][+|-]?[0-9]+)?` is floating point number.

## Deterministic Finite Automata (DFA)
Deterministic Finite Automata (DFA) is a state machine *without ambiguity* (the behavior of the machine is repeatable using the same string).
* Deterministic means the machine is in a state. Upon receipt of a symbol, it will go to a unique state.
* Finite: have a finite number of states.
* Automata: it is a self-operating machine.

A language is called a **regular language** if some finite automaton *recognizes* it.

An example of DFA is below:

![Alt text]({{ "/assets/posts/udacity-compiler-course/DFA.png" | absolute_url }})

* There are two types state, rejecting state (single circle) and accepting state (double circle), meaning to accept or reject the string if the automaton ends in it.
* The incoming arrow indicates the start state.

The formal definition of DFA: a DFA consists of:
1. Alphabet $$\Sigma$$;
2. A set of states $$Q$$;
3. A transition function $$\delta: Q \times \Sigma \rightarrow \Sigma$$;
4. One start state $$q_0$$;
5. One or more accepting states: $$F \subseteq Q$$.

The language accepted by a DFA is the set of strings such that the DFA ends at an accepting state.
* Each string is $$c_1c_2...c_n$$ with $$c_i \in \Sigma$$;
* States are $$q_i = \delta(q_{i-1}, c_i)$$ for $$i = 1, ..., n$$;
* $$q_n$$ is an accepting state.

## Nondeterministic Finite Automata (NFA)
Nondeterministic Finite Automaton (NFA) differs from DFA in three ways:
1. Same input may produce multiple parts;
2. Allows transition with an empty string. $$q_1$$ can transition into $$q_2$$ on empty string $$\epsilon$$, also knows as "cloning".
3. Transition from one state to several different states on a given character. In other words, the NFA clones itself and operates from different states on a given character.

The NFA starts in the start state. When encountering transitions on $$\epsilon$$ or multiple transitions on the same character, it clones itself. If no legal transition from the state on the incoming alphabet, the machine dies. If one copy of the NFA ends an accepting state, then the NFA accepts the string.

The formal definition of NFA: a NFA consists of:
1. Alphabet $$\Sigma$$;
2. A set of states $$Q$$;
3. A transition function $$\delta: Q \times \Sigma_\epsilon \rightarrow P(Q)$$ where $$\Sigma_\epsilon = \Sigma \cup \{\epsilon\}$$ and $$P(Q)$$ is the power set of $$Q$$;
4. One start state $$q_0$$;
5. One or more accepting states: $$F \subseteq Q$$.

The only difference between DFA and NFA is the definition of the transition function.

The language accepted by a NFA is the set of strings such that the NFA ends at an accepting state.
* Each string is $$c_1c_2...c_n$$ with $$c_i \in \Sigma_\epsilon$$;
* States are $$q_i \in \delta(q_{i-1}, c_i)$$ for $$i = 1, ..., n$$;
* $$q_n$$ contains an accepting state.

## Relationship between DFA and NFA
>Theorem: For every language L accepted by an NFA, there is a DFA that accepts L.

In other words, DFA and NFA are equivalent computational models. The proof idea is to combine the set of states in an NFA that a substring can reach and produce a corresponding state in the DFA. By iterating through the string, we can produce a corresponding DFA from the NFA.

## Closure
> Closure property: certain operations performed on languages in a class will produce results in the same class.

Regular languages are closed under regular operations:
* Union:

$$L_1 \cup L_2=\{x|x\in L_1 \,\textrm{or}\, x\in L_2\}$$

* Concatenation:

$$L_1 \dot L_2=\{xy|x\in L_1 \,\textrm{and}\, y\in L_2\}$$

* Star/repetition:

$$L_1*=\{x_1...x_k|x_j\in L_1 \,\textrm{for all}\, 1\leq j\leq k, k \geq 0\}$$

To prove operations are regular, assuming $$M_1$$ and $$M_2$$ are NFA for $$L_1$$ and $$L_2$$, respectively, we can construct a NFA $$M_3$$ such that $$L(M_3)$$ equals the result of the regular operations.

* Union: Add a start state with $$\epsilon$$ transitions into either $$M_1$$ or $$M_2$$. Therefore, if a string is in $$L_1$$ then the clone to $$M_1$$ will end in accepting state.
![Alt text]({{ "/assets/posts/udacity-compiler-course/closure-union.png" | absolute_url }})

* Concatenation: Add $$\epsilon$$ transitions from accepting states in $$M_1$$ to the start state in $$M_2$$.
![Alt text]({{ "/assets/posts/udacity-compiler-course/closure-concatenation.png" | absolute_url }})
* Start: Add a new start state that is an accepting state (for empty string) with $$\epsilon$$ transition to the original start state, and add $$\epsilon$$ transitions from accepting states to the original start state.
![Alt text]({{ "/assets/posts/udacity-compiler-course/closure-star.png" | absolute_url }})

Therefore, we can use regular operations to construct larger NFAs from simple NFAs.

Because a regular language has some DFA recognizes it, DFA can be converted from NFA, and NFA can be constructed from regular operations (i.e. regular expressions), we can conclude:

> A language is regular **if and only if** some regular expression describes it.

## Building Lexical Analysis
To convert a language specification into code:
* Write down the Regular Expression for the input language;
* Build a big NFA;
* Build the DFA that simulates the NFA;
* Systematically shrink the DFA;
* Turn it into code.

### RE to NFA: Thompson's Construction
Key idea:
* Construct NFA patterns for each symbol and each operator. For example, the NFA for character `a` is as follows.
![Alt text]({{ "/assets/posts/udacity-compiler-course/thompsons-construction-symbol.png" | absolute_url }})

* Join them with $$\epsilon$$ moves by using the precedence order. For example, the NFAs for character `ab` and `a*` are as follows.
![Alt text]({{ "/assets/posts/udacity-compiler-course/thompsons-construction-concatenation.png" | absolute_url }})
![Alt text]({{ "/assets/posts/udacity-compiler-course/thompsons-construction-star.png" | absolute_url }})

A more complex example of `(b|c)*`:
![Alt text]({{ "/assets/posts/udacity-compiler-course/thompsons-construction-join.png" | absolute_url }})

### NFA to DFA: Subset Construction
Two key functions:
* $$Move(s_i,\alpha)$$ is the set of states reachable from $$s_i$$ by $$\alpha$$.
* $$\epsilon -closure(s_i)$$ is the set of states reachable from $$s_i$$ by $$\epsilon$$.

The idea of the algorithm is
1. Start state $$S_0$$ is derived from $$s_0$$ of the NFA: $$S_0=\epsilon-closure({s_0})$$.
2. Take the image of $$S_0$$, $$Move(S_0,\alpha)$$ for each $$\alpha\in \Sigma$$, and take its $$\epsilon-closure$$.
3. Iterate until no more states are added.

The algorithm is
![Alt text]({{ "/assets/posts/udacity-compiler-course/subset-construction-algo.png" | absolute_url }})

Note:
1. $$W$$ is the list of states to explore;
2. $$T$$ is the transition function.

A DFA state is an accepting state if its $$\epsilon$$-closure contains an NFA accepting state.

Subset construction is an example of **fixed-point computation**:
* *Monotone construction* of some finite set;
* *Halts* when it stops adding to the set.

### DFA Minimization
The goal of DFA minimization is to reduce the size of the transition table.

> Observation: Subset construction algorithm merges prefixes in the NFA.

However, it does not merge suffixes. To do so, use subset construction twice.

For an NFA $$N$$:
* Let $$reverse(N)$$ be the NFA constructed by making initial state final (and vice versa) and reversing the edges.
* Let $$subset(N)$$ be the DFA that results from applying the subset construction to $$N$$.
* Let $$reachable(N)$$ be $$N$$ after removing all states that are not reachable from the initial state.

Then

$$reachable(subset(reverse(reachable(subset(reverse(N))))))$$

is the minimal DFA that implements $$N$$. This essentially is to merge suffixes first and merge prefixes second by subset construction.

# Parsing
### Content-Free Grammar and Ambiguity
Context-free grammars (CFGs) can describe all regular languages. It is easy to derive strings $$w \in L(G)$$ from a CFG $$G$$ (generative aspect of CFG). The interesting problem for compilers is the analytical aspect: Given a CFG $$G$$ and a string $$w$$, decide if $$w\in L(G)$$ and if so, how do you determine the **derivation tree** or the sequence of rules that produce $$w$$. This is the problem of parsing. In other words, the parser tries to choose the right sequence of rules from $$G$$ that produces $$w$$.

To derive a string, the parser can go from left or right. The left-most derivation is at each step, to replace the leftmost non-terminal symbol with a derived rule. The right-most derivation expands the rightmost non-terminal symbol at each step. The leftmost derivation is the most common since tokens are generated from left to right.

For a CFG $$G=(V, \Sigma, P, S)$$,

* $$V$$ is the set of nonterminals/variables, each represents a different type of phrase or clause in the sentence.
* $$\Sigma$$ is the set of terminals, making up the actual content of the sequence.
* $$P$$ is the set of production rules from $$V$$ to $$(V\cup \Sigma)^*$$, where $$**$$ is the Kleene star operation.
* $$S$$ is the start symbol, used to represent the whole sentence. $$S\in V$$.

A **derivation tree** has the following properties:
1. The root of the tree is labeled $$S$$.
2. Each leaf node is in $$\Sigma \cup \{\epsilon\}$$.
3. Each interior node is in $$V$$.
4. If node has label $$A\in V$$ and its children $$a_i,...,a_n$$ (from left to right), then there exists a rule $$r=A\rightarrow a_i,...,a_n$$, $$r\in P$$, and $$a_j\in V \cup \Sigma \cup \{\epsilon\}$$. A leaf labeled $$\epsilon$$ is a single child (i.e. has no siblings).

Let $$G$$ be a CFG. We have $$w\in L(G)$$ is and only if there exists a derivation tree of $$G$$ that yields $$w$$.

A string $$w\in L(G)$$ is derived **ambiguously** if it has more than one derivation tree. Or equivalently, if it has more than one leftmost derivation (or rightmost). A grammar is ambiguous if some strings are derived ambiguously.

To resolve ambiguity,
1. Define precedence. Group operators into different groups. Operators with lower precedence are closer to the root, or are expanded from low precedence to high precedence.
2. Left-association: perform operations of same precedence from left to right (left associativity).
3. Parenthesization.

(Abstract) Syntax Trees are simplified representations of parse trees by collapsing terminals into the non-terminal parents.

### Recursive Descent Parsing
There are two classes of parsers:
* LR Parser is the bottom-up parser (i.e. reduces the sentence to the start symbol). Letter "L" means it scans from left to right, letter "R" means it traces rightmost derivation of input string.
* LL parser is the top-down parser (i.e. expands the start symbol to the sentence). The second "L" means it traces leftmost derivation of the input string.

Typically, parsers are annotated as LL(k) where `k` refers to the maximum number of tokens it looks ahead to uniquely choose a rule. The lower `k` is, the higher the efficiency is. Deterministic parser is also called non-backtracking parser, and it's more efficient as the parsing time is bounded.

For each non-terminal variable, the parser correctly selects a rule for its expansion that matches the incoming token(s). If no such rules exist, throw an error. The output is the abstract syntax tree.

Backus-Naur form (BNF) is a notation technique for context-free grammars. It looks like this:
![Alt text]({{ "/assets/posts/udacity-compiler-course/BNF.png" | absolute_url }})

To better describe the left-recursion that an expression can be expanded into a sentence including itself, we can use Extended Backus-Naur form (EBNF). EBNF provides, among other things, regex-like symbols to describe repetition using `{}` and `*`. Any grammar defined in EBNF can also be represented in BNF, but the EBNF is more succinct. EBNF can be converted to BNF. For example,

```
// Original BNF
<term> ::= <term> '*' <factor> | <factor>
// EBNF
<term> ::= <factor> { '*' <factor> }
// EBNF to BNF
<term> ::= <factor> <t_tail>
<t_tail> ::= '*' <factor> <t_tail> | ε
```

You can notice that through EBNF to BNF conversion, all the left recursion are converted into right recursion. This process introduces no ambiguity.

We can implement the following EBNF:

```
<expr> ::= <term> { <addop> <term> }
<addop> ::= '+' | '-'
```

iteratively:
```c
// peakToken() is to get the next token without consume it.
// matchToken() is to consume the token.
// PLUS = '+' and MINUS = '-'.
void expr() {
    term();
    int token;
    while((token = peakToken()) == PLUS || token == MINUS) {
        matchToken(token);
        term();
    }
}
```

The EBNF can be converted into the following BNF:

```
<expr> ::= <term> <e_tail>
<e_tail> ::= <addop> <term> <e_tail> | ε
<addop> ::= '+' | '-'
```

and the BNF can be implemented recursively:
```c
void expr() {
    term();
    e_tail();
}

void e_tail() {
    int token;
    if((token = peakToken()) == PLUS || token == MINUS) {
        matchToken(token);
        term();
        e_tail();
    } else return;
}
```  
When a function returns successfully, an edge from the caller (parent) to the callee (child) is added to the parse tree.

To use recursive descent parsing, the left recursion in a grammar must be re-written into right recursion. BNF to ENBF back to BNF above does exactly this. We can directly convert left-recursion into right-recursion if it is well-defined.

Immediate left recursion `A ::= A u | v` where `v` does not start with `A` can be converted into:

```
A ::= v A'
A' ::= u A | ε
```

The general immediate left recursion:
```
A ::= A u1 | A u2 | ... | A un | v1 | v2 | ... | vm
```
where `vi` does not start with `A` can be converted into

```
A ::= v1 A' | v2 A' | ... | vm A'
A' ::= u1 A' | u2 A' | ... | un A' | ε
```

For example,

```
<exp> ::= <exp> '+' <term> | <exp> '-' <term> | <term>
```

can be converted into

```
<exp> ::= <term> <exp'>
<exp'> ::= '+' <term> <exp'> | '-' <term> <exp'> | ε
```

In some cases, multiple rules may sure the same prefix. This is inefficient as it requires more look-aheads (i.e. increase `k`). A common practice is to rewrite the rule:

```
// original
A ::= uv | uw
// new
A ::= uA'
A' ::= v | w
```

With recursive descent parsing, we can generate a parse tree. To convert a parse tree to the abstract syntax tree, we need
1. Remove all $$\epsilon$$ edges.
2. Recursively remove parents with an out degree of 1.
3. Remove intermediate non-terminal nodes to connect the token to the sub-tree root node for derivation. The operator is placed to the sub-tree root.

Or the syntax tree can be generated while parsing, called syntax-driven directed construction. For example, the parser for this syntax
```
<expr> ::= <term> { <addop> <term> }
<addop> ::= '+' | '-'
```

can generate the syntax tree during parsing:

```
SyntaxTree *expr() {
    SyntaxTree *temp = term();
    int token;
    while((token = peakToken()) == PLUS || token == MINUS) {
        matchToken(token);

        SyntaxTree *tree = makeOpNode(token);
        tree->left = temp;
        tree-> right = term();
        temp = tree;
    }
    return temp;
}
```

## Top Down Parsing: LL Parser
LL Parser works well with scanner as it performs leftmost derivation. LL(1) Parser matches the rule with the incoming token.
1. Push the start symbol onto a stack.
2. Replace the non-terminal symbol on top of the stack using grammar rules. The leftmost symbol of the rule output is the top of the stack.
3. If the top of stack matches the input token, both are to be discarded (i.e. put into syntax tree); mismatch is a syntax error.
4. Eventually, both stack and the input string are empty. Then it is a successful parse.

LL(1) Parser is implemented with the parsing table that instructs how non-terminals should be expanded based on the input token. LL(1) Parser is deterministic; in other words, a single rule for expansion is selected by 1 token lookahead (i.e. the input token). LL(1) parse table consists of *a column for each token and a row for each non-terminal symbol*. A grammar is LL(1) grammar if the associated LL(1) parsing table has *at most one* production rule in each table entry. LL(1) grammar is a proper subset of context-free grammar.

To generate the parse table from the grammar, we look into two concepts: first set and follow set.

First set for a symbol (on the left hand side of a rule), $$First(X)$$, is the set of tokens that we find at the beginning of the right hand side of the rule. This is done by recursively expanding the rules in all possible ways and, due to right recursion, the beginning of the right hand side will eventually be a token.

* If $$X$$ is a terminal or $$\epsilon$$, then $$First(X)=\{X\}$$.
* If $$X$$ is non-terminal, then for each production rule $$X \rightarrow X_1X_2...X_n$$, $$First(X)$$ contains $$First(X_1) - \{\epsilon\}$$.
* If for some $$i<n$$, $$First(X_1),...,First(X_i)$$ all contain $$\epsilon$$ (i.e. the first $$i$$ symbols can be empty), $$First(X)$$ contains $$First(X_{i+1}) - \{\epsilon\}$$.
* If $$First(X_1),...,First(X_n)$$ all contain $$\epsilon$$ (i.e. all symbols can be empty), $$First(X)$$ contains $$\epsilon$$.

The first set algorithm is below:
![Alt text]({{ "/assets/posts/udacity-compiler-course/first-set-algorithm.png" | absolute_url }})

Follow set of $$A$$ is those symbols which will follow after $$A$$ and is used to determine if a rule such as $$A\rightarrow \epsilon$$ should be invoked to remove $$A$$ and expose the tokens that follow $$A$$ for matching them.

* If $$A$$ is the start symbol, then $$\$$$ (the end of string) is in $$Follow(A)$$.
* If there is a production rule $$B\rightarrow \alpha A\beta$$, then $$Follow(A)$$ contains $$First(\beta)-\{\epsilon\}$$. This is because $$\beta$$ is right after $$A$$, any symbols at the beginning of $$\beta$$ must appear immediately after $$A$$.
* If there is a production rule $$B\rightarrow\alpha A\beta$$ and $$\beta$$ is nullable (i.e. there exists a rule $$\beta\rightarrow\epsilon$$), then $$Follow(A)$$ contains $$Follow(B)$$. This is because anything that follow after $$B$$ can follow after $$A$$ by letting $$\beta$$ be $$\epsilon$$.

Note:
* $$\$$$ is needed to indicate the end of the string.
* $$\epsilon$$ is never a member of a folow set.

The follow set algorithm is below:
![Alt text]({{ "/assets/posts/udacity-compiler-course/follow-set-algorithm.png" | absolute_url }})

To build the parse table with first sets and follow sets, repeat the following two steps for each nonterminal $$A$$ and production rule $$A\rightarrow\alpha$$:
1. For each token $$a\in First(\alpha)$$, set the entry $$M[A,a]=A\rightarrow\alpha$$.
2. If $$\epsilon\in First(\alpha)$$, for each $$a\in Follow(A)$$ (a token or $$\$$$), set the entry $$M[A,a]=A\rightarrow\alpha$$.

To get the parse table for a grammar, perform the following steps:
1. Convert to right recursion if necessary.
2. Compute the first sets and follow sets for each symbol.
3. For each rule, compute the predict set:
$$
Predict(A\rightarrow\alpha)=\begin{cases}
First(\alpha),&\text{if }\epsilon\notin First(\alpha) \\
First(\alpha)-\{\epsilon\} \cup Follow(A), &\text{otherwise}
\end{cases}
$$
4. If a token $$t$$ appears in $$Predict(A\rightarrow\alpha)$$, put rule $$A\rightarrow\alpha$$ in entry $$M[A,t]$$.

# Semantic Analysis
Semantic analysis, unlike lexical or syntax analysis, is context-sensitive analysis. There are two categories of solutions:
1. Formal methods, such as context-sensitive grammars (too expensive), attribute grammars (adding and propagating attributes during parsing).
2. Ad-hoc techniques, such as symbol tables, action routines (specific to language).

We will study the formalism, an attribute grammar. It clarifies many issues in a succinct and immediate way, separates analysis problems from their implementations (i.e. no action routines). But it has limitations and inefficiencies so many modern compilers incorporate ad-hoc techniques.

An attribute grammar is a context-free grammar augmented with a set of rules. Each symbol in the derivation (or parse tree) has a set of named values aka attributes. The rules specify how to compute each attribute. Attribution rules are functional; they uniquely define the attribute value.

Attribution rules plus parse tree imply an attribute dependence graph, as some attributes are computed by propagating down the tree, while others are computed by propagating up the tree. Multiple evaluation orders are possible. The data-flow model for evaluation suggests to evaluate independent attributes first, others in order as input values are evaluated. However, the data-flow model is inefficient as it requires tracking of what attributes are evaluated.

Attributes can be classified into two categories:
1. Synthesized attribute: depends on values from children. They can be computed in a single bottom-up pass.
2. Inherited attribute: depends on values from siblings and/or parent. They can be computed top-down, but not easily done at parse time. One example of inherited attribute is the value definition in a block structure.

Note, Attribute Grammar is a specification for the computation, not an algorithm.

There are multiple evaluation methods.
1. Dynamic, dependence-based methods:
   1. Build the parse tree;
   2. Build the dependence graph;
   3. Perform a topological sort of the dependence tree.
   4. Define attributes in the topological order.
1. Rule-based methods (e.g. tree walk):
   1. Analyze rules at compiler-generation time.
   2. Determine a fixed (static) ordering of evaluations.
   3. Evaluate nodes in that order.
1. Oblivious methods (e.g. passes, dataflow):
   1. Ignore rules and parse tree.
   2. At design time, pick a convenient order and use it.

The dependence graph must be acyclic. However, general circularity testing problem is inherently exponential.

We can prove that some grammars can only generate acyclic dependence graphs, and the largest such class is "strongly non-circular" grammars (SNC). SNC grammars can be tested in polynomial time. There are many evaluation methods to discover circularity dynamically, but it is a bad property for a compiler to have. Therefore, we should use provably noncicular grammars.

# Intermediate Representation (IR) Code Generation
There are three major categories:
* Structured (graph-oriented), e.g. trees, DAGs. It is heavily used in source-to-source translators and tends to be large.
* Linear. It is pseudo-code for an abstract machine with variable level of abstraction. It can leverage simple and compact data structures. Examples include 3 address code (operand 1, 2, result) and stack machine code (like Java bytecode).
* Hybrid. The combination of graphs and linear code. For example, control-flow graph.

High IR captures high-level language constructs and Low IR captures low-level machine features. Thus, we need a systematic algorithm, called *lowering*, to translate from high-level IR to low-level IR. The general idea is to
1. define the translation for each AST node, assuming we have the code for its children;
2. come up with a scheme to stitch them together;
3. recursively descend the AST.
