# 02. Lexical Analyzer 1

Lexical analysis is the first phase of a compiler, responsible for breaking down the source code into meaningful tokens. This process transforms a character stream into a token stream that can be processed by the parser.

## Starters

- Structure of Compilers
- Lexical analyzer (scanner): groups characters into tokens (meaningful sequences)
- Examples of Tokenization (Practice)

## Overview

To build a scanner, we need to learn:

- **Concepts**: Tokens, Patterns, Lexemes
- **How to depict patterns**:
    - Regular Languages
    - Operations on strings & languages
    - Regular Expressions (REs)
- **How to recognize patterns**:
    - Finite Automata (DFAs, NFAs)
- **How to implement a scanner**:
    - Token patterns → REs
    - REs → NFAs: Thompson's Construction
    - NFAs → DFAs: Subset Construction

## Tokens, Patterns, and Lexemes

### Definition {id="definition_1"}

A token is a basic unit of syntax in programming languages. Formally, a token is represented by a tuple ``:
- `token_name` represents a type of lexical unit
- `value` is optional; it contains the attribute values of the token

For example, in the expression `a = b * 2`, the tokens would be:
```
<id, a> <assign> <id, b> <multiply> <number, 2>
```

### Classes of token_name

- **Keywords**: pre-defined strings (if, else, float)
- **Identifiers**: user-defined identifiers
- **Constants**: numeric constants
- **Operators**: assign (=), comparison (, ==)
- **Punctuation symbols**: parentheses, commas
- **Whitespaces**: blank, tab, newline

A pattern is a rule describing the set of strings of a particular token, while a lexeme is a sequence of characters that matches the pattern for a token (an instance of a token).

## Role of Lexical Analyzer

The lexical analyzer:
- Recognizes correct tokens from the source program
- Passes tokens to the parser (syntax analyzer)
- Enters discovered lexemes into the symbol table for parser use

## Specification of Tokens

Regular languages provide a simple yet powerful way to describe patterns of tokens.

## Alphabets, Strings, and Languages

- **Alphabet**: a finite, non-empty set of symbols
- **String**: a finite sequence of symbols from the alphabet
- **Language**: a set of strings over an alphabet

### String Operations

| Notation | Name           | Definition                       | Examples |
|----------|----------------|----------------------------------|----------|
| s        | String         | A finite sequence of symbols     |          |
| \|s\|    | Length         | Number of symbols in s           |          |
| s₁s₂     | Concatenation  | Writing one string after another |          |
| ε        | Empty String   | A string with no symbols         |          |
| sⁿ       | Exponentiation | Concatenation of s for n times   |          |

### Language Operations

| Notation | Name             | Definition                            |
|----------|------------------|---------------------------------------|
| L        | Language         | A set of strings on an alphabet       |
| ∅        | Empty set        | A set without any elements            |
| L₁ ∪ L₂  | Union            | s ∈ L₁ or s ∈ L₂                      |
| L₁ ∩ L₂  | Intersection     | s ∈ L₁ and s ∈ L₂                     |
| L₁ - L₂  | Difference       | s ∈ L₁ and s ∉ L₂                     |
| L₁L₂     | Concatenation    | {xy \| x ∈ L₁, y ∈ L₂}                |
| L*       | Kleene closure   | Concatenation of L zero or more times |
| L⁺       | Positive closure | Concatenation of L one or more times  |

## Regular Expressions (REs)

Regular expressions are a powerful way to describe patterns of tokens. They combine:
- Strings of symbols from alphabet
- Parentheses
- Concatenation
- Operators (union and Kleene-closure)

### Definition {id="definition_2"}

Let Σ be a given alphabet. Then:
1. ∅, ε, and a (for all a ∈ Σ) are all regular expressions (primitive REs)
2. If r₁ and r₂ are regular expressions, so are:
    - (r₁)
    - r₁|r₂ (union)
    - r₁r₂ (concatenation)
    - r₁* (Kleene closure)

### Precedence Rules

To disambiguate REs, the precedence order is:
1. Parentheses
2. Kleene closure (*)
3. Concatenation
4. Union (|)

### Extensions of REs (Shorthand Notations)

- One or more instances: r⁺ = rr*
- Zero or one instance: r? = (r|ε)
- Character classes: [abc] = (a|b|c)
- Range notation: [a-z] = (a|b|...|z)

## Finite Automata

An automaton is an abstract model of a digital computer. Finite automata are one of the simplest forms of automata, with:
- A finite number of internal states
- Input processing consisting of a sequence of symbols
- Transitions between states based on current state and input

### Definition

A finite automaton (FA) is defined by the tuple M = (Q, Σ, δ, q₀, F) where:
- Q is a finite set of internal states
- Σ is the input alphabet
- δ is the transition function
- q₀ ∈ Q is the initial state
- F ⊆ Q is a set of final states

## DFAs vs. NFAs

### Deterministic Finite Automata (DFAs)
- Exactly one transition for each state and input symbol
- No ε-transitions allowed
- Easy to implement and execute

### Non-deterministic Finite Automata (NFAs)
- Multiple transitions for each state-input pair allowed
- ε-transitions are allowed
- Undefined transitions possible
- Simpler to express from REs

DFAs and NFAs are equally powerful - for any NFA, we can find an equivalent DFA, and vice versa.

## Thompson's Construction (RE → NFA)

Thompson's Construction is an algorithm to convert REs to NFAs:
1. Construct NFA for each symbol and operator of the RE
2. Glue them with ε-transitions in precedence order of RE operators

### Inductive Basis
NFAs for primitive regular expressions (∅, ε, a)

### Inductive Step
For REs r₁ and r₂ with corresponding NFAs N₁ and N₂, we can build NFAs for:
- (r₁)
- r₁|r₂ (union)
- r₁r₂ (concatenation)
- r₁* (Kleene closure)

## Subset Construction (NFA → DFA)

The Subset Construction algorithm converts an NFA to an equivalent DFA:
1. Each state in the DFA corresponds to a set of states in the NFA
2. The initial state of the DFA is the ε-closure of the initial state of the NFA
3. For each state S in the DFA and each input symbol `a`, compute the ε-closure of the set of states reachable from S on input a

## Implementing Lexical Analyzers

To convert token specifications to code:
1. Define regular expressions for each token type
2. Convert REs to NFAs using Thompson's Construction
3. Convert NFAs to a DFA using Subset Construction
4. Implement the DFA in code

When implementing a scanner, two important rules are followed:
1. The longest possible match is taken
2. There is a priority among patterns (e.g., keywords take precedence over identifiers)
