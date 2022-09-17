---
layout: default
---

# CPSC 421

## Overview of the Course
Broad categories of lectures:
- Automata Theory: How to parse languages?
- Computability Theory: Can all software be verified?
- Midterm
- Complexity Theory: Do all problems have efficient algorithms?
	- Ex. Prime number factoring, travelling salesman problem
- Communication Complexity: How many bits must we send?
	- Ex. ???

## Why study models of computation?
- Helps us understand what is/isn't possible
- Helps us think about unconventional computing methods (brains, molecules, etc)

## Class Logistics
### Prereqs
- Proofs, by induction especially
- CPSC 213 and 221 (oop)
- Sets, graphs, discrete math, a little bit of linear algebra
### Assignments
- Assignments on Piazza (?)
- Assignment #1 should take an hour
- In total: 10 assignments
- Typically hard, includes bonus questions
- Groups of 2 allowed
- LaTeX, Gradescope

Main concept of the course: **Computational Problems**

We want to understand things like: can I solve this problem? can I solve it fast?
- Ex. 1: Given a list, sort it.
- Ex. 2: Given an integer, find its prime factors.
- Ex. 3: Given a polynomial, find its roots.

## Some Notation

Representation: Everything is a string (??? - what was the context of this?).
To first talk about strings, we need to define an _alphabet_.

- Definition: An **alphabet** is any finite non-empty set.
	- Usually represented as $\Sigma$, or $\Gamma$. Often $\Sigma = \{0, 1\}$, or $\Sigma = ASCII$, or $\Sigma = Unicode$.
- Definition: A **string** is a finite sequence of 0+ characters from the alphabet.
- Definition: We often will talk about the **empty string**, so we could use a symbol for it: we will call it $\epsilon$.
- Definition: $\Sigma*$ is the set of all strings with alphabet $\Sigma$.
- Definition: A **language** is any set (can be infinite) $L \sube \Sigma*$. Even $L = \emptyset$ is okay.
- Definition: A **computational problem** is a function mapping strings to strings.
	- Ex. $f("6") --> "2,3"$, $f("30") --> "2,3,5"$, $f("z8sst") --> "error"$

The definition of a computational problem does not specify **how** to produce the outputs from the inputs. No algorithm here.

We're often interested in a specific type of computational problem: **decision problems**. A decision problem is a computational problem with a boolean/binary/y-n output.
-- Decision

We can usually tweak computational problems to become decision problems: eg. given (x,y), does x have a prime factor that is < y?

Important Idea: decision problems are equivalent to languages.
Example: 
- Let f be a decision problem. 
- Let L = the set of all strings that f maps to Yes.
- $L = \{ x \in \Sigma* : f(x) = "Yes" \}$.
- $T = \{ x \in \Sigma* : f(x) = "No" \}$.
- L and T are complementary sets, no element is in both and every element (of the overarching language) is in one or the other.
Another example:
- $L = \{ x \in \Sigma* : \text{x is prime} \}$
- The decision problem is $f(x) = \{ \text{Yes if x is prime}; \text{No otherwise} \}$

Set builder notation: $\{ x \in \Sigma* : f(x) = "Yes"$ reads as for all X in $\Sigma*$ _such that_ $f(x)$ is equal to "Yes"

## Today's Learning Goals (recap? next day?)
- Note: This course and its slides were created for a MWF class, haven't yet been adapted for TT
- What is the theory of computation about?
- What is a comptational problem?
What is computation?
- **Definitions**: alphabet, string, comptuational problem, decision problem, language, Finite Automaton (of DFA), "Language Accepted By", Regualar Language

