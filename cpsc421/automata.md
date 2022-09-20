---
layout: default
title: Automata Theory
---

How to parse languages?

## (Deterministic) Finite Automata
Informally: a computational machine for a decision problem. Takes an input string, and:
- Outputs Accept + stops
- Outputs Reject + stops
- Or runs forever without outputting anything.

Definition: Given a computational machine M, the _language recognized by M_ is $L(M) = \\{ x \in \Sigma* : \text{machine M accepts x} \\}$.

Let's talk about a different language: $L'(M) = \\{ x \in \Sigma* : \text{machine M rejects x} \\}$. These are **not** complements: in our model of computation the machine can run forever.

Here's an example of a finite automata:
```automata
--> q1: 0->q1, 1->q2
acc q2: 1->q2, 0->q3
q3: 0,1->q2
```

Notation for finite automata:
- Leading in arrow is the initial state: `-->`
- Double circle for accepting state: `acc`

Formal definition of a **(deterministic) finite automata**
- Definition: A finite automation is a 5-tuple $M = (Q,\Sigma,\delta,q_0,F)$ where
- $Q$ is the **set of states**; non-empty and finite
- $\Sigma$ is our **alphabet**; non-empty and finite
- $\delta : Q \times \Sigma \rightarrow Q$ is the **transition function**
- $q_0 \in Q$ is the **start state**
- $F \subseteq Q$ is the set of **accepting states**

Defining $\delta$
- **States**: $Q = \\{q_1,q_2,q_3\\}$
- **Alphabet**: $\Sigma = \\{0,1\\}$
- **Start State**: $q_0 = q_1$
- **Accepting States**: $F = \\{q_2\\}$
- **Transition Function**: $\delta (q,s) = \ldots$ A 2x3 table of s and q

What does it mean for a finite automaton to accept a given string?
- Definition: A finite automation accepts a string $w = w_1 w_2 \ldots w_n$ if there exists a sequence $r_0, \ldots r_n \in Q$ with:
	- $r_0 = q_0$
	- $r_i = \delta(r_{i-1},w_i)$ for $i=1,\ldots,n$
	- $r_n \in F$

$L(M) = \\{w : \text{M accepts w}\\}$ is the language recognized by $M$.

Example:
```automata
L(M) = \emptyset
--> q0: 0->q0, 1->q0
```

Another example:
```automata
L = {11011,110011,1100011,...}
--> q1: 1->q2
q2: 1->q3
q3: 0->q4
q4: 0->q4, 1->q5
q5: 1->q6
acc q6
qerror: all unspecified transitions
```

Imagine making a $q_{error}$: any transition not specified goes to $q_{error}$.

## Regular Languages

- Definition: L is a **regular language** if some finite automation recognizes L.
	- The canonical example for a _non_-regular language is $L = \\{0^i 1^i : i \geq 0 \\}$

Regardless of if we have an empty string as the input, we will still start off in $q_0$.

### Operations on Regular Languages

- Definition: For strings x and y, their **concatenation** is denoted $x \circ y$, or xy. For languages A and B, their concatenation is $A \circ B = AB = \\{ a \circ b : A \in A, b \in B\\}$
- Theorem: If A and B are regular, then $A \circ B$ is also regular.

Example:
- $L_1 = \\{\epsilon, 0, 00, 000, \ldots \\}$
- $L_2 = \\{11011, 110011, 1100011 \ldots \\}$
- $M_1$ is an accepting state $q_1$ with $0->q_1$
- $M_2$ was previously defined

Point is, it can get difficult to concatenate arbitrary DFAs (concatenating the DFA examples were done in relatively different ways)

## Nondeterministic (Finite Automata)

Informally: Giving our computational model the additional power to make "lucky guesses", i.e. arbitrary choices

Brief definition: An **NFA** is similar to a DFA, except $\delta : Q \times (\Sigma \cup \\{\epsilon\\}) \rightarrow 2^Q$
- $2^Q = \\{\text{set of all subsets of Q}\\}$
- input 1: a state
- input 2: a character, or nothing
- input 3: a set of states

Nondeterminism is in the name: notably, this means that
- you can have multiple arrows with the same label
- the output is a _set_ of states
- There can be $\epsilon$s that skip states: these are equivalent to additional (carefully placed) arrows

NFA Example
```automata
--> s1: 0,1->s1, 1->s2
s2: 0,\epsilon->s3
s3: 1->s_4
acc s4: 0,1->s4
```
- $Q = \\{s_1,s_2,s_3,s_4\\}$
- $\Sigma = \\{0,1\\}$
- $q_0 = s_1$
- $F = \\{s_4\\}$

Formal definition of a **(nondeterministic) finite automata**
- Definition: A finite automation is a 5-tuple $M = (Q,\Sigma,\delta,q_0,F)$ where
- $Q$ is the **set of states**; non-empty and finite
- $\Sigma$ is our **alphabet**; non-empty and finite
- $\delta : Q \times (\Sigma \cup \\{\epsilon\\}) \rightarrow 2^Q$ is the **transition function**
- $q_0 \in Q$ is the **start state**
- $F \subseteq Q$ is the set of **accepting states**

(only thing changed is the transition function)

Recall: What does it mean for a DFA to accept?

$M$ accepts $w = w_1, w_2, \ldots, w_n$ if there exists a sequence $r_0, \ldots, r_n \in Q$ for which:
- $r_0 = q_0$
- $r_i = \delta(r_{i-1}, w_i)$ for all $i = 1..., n$
- $r_n \in F$

What does it mean for an NFA to accept?

The NFA $M$ accepts the string $w$ if there exists a string $y_1,y_2, \ldots, y_m \in (\Sigma \cup \\{\epsilon\\})* \ldots$

(missed some stuff)

### Some important theorems about finite automata

- Theorems 1.39 and 1.40: The class of all languages recognized by DFAs == the class of all languages recognized by NFAs == regular languages.
	- This "containment" is trivial, $\text{DFAs} \sube \text{NFAs}$
- Theorems 1.25 and 1.45: If A and B are regular then $A \cup B$ is regular.

## Closure of Regular Languages

### Closure under union
$C = (A \cup B) = \\{x : x \in A \lor x \in B\\}$

Closure under union: When you make the union, have an epsilon transition to A and also to B.

### Closure under concatenation
$A \circ B = \\{xy: x \in A \land y \in B \\}$

We want to kind of glue them together to get a machine that recognizes their concatenation: so how do we do this?

For every accepting state of A, add an epsilon to the start state of B, and remove the acceptingness of the accepting states in A.

(This is a convoluted explanation. I should just embed an image.)

Theorem: Let $A,B$ be regular. The following languages are also regular:
- $A \cup B$
- $A \circ B = \\{a \circ b : a \in A, b \in B\\}$
- $A* = \\{x_1 \circ x_2 \circ \ldots \circ x_u : k \geq 0 \forall x_i \in A\\}$
- Complement of A (the set of all strings not in A): $\Sigma* \mid A$.

### Closure under Kleene star
$C = A* = \\{x_1 x_2 x_3 \ldots x_k : k \geq 0 \land x_i \in A \\}$

The Kleene star generally represents the set of all strings that can be made with a language.

A is a machine that accepts a string x, A* is a machine that accepts multiple concatenated states $x_k$

Remember, epsilons are _really arbitrary_ - the professor is describing them as "lucky guesses".

All you need to do to have a DFA that recognizes the complement is to switch the accepting and the nonaccepting states.

### epsilon-closures

- Theorem: Let M be an NFA. Then there is a DFA M' such that L(M') = L(M).
- Definition: For any set $S \sube Q$, the set E(S) is all states reachable from S by following _any number_ (including zero) of arrows labelled by $\epsilon$. Called the $\epsilon$-closure.
	- Note: $S \sube E(S)$

Main idea: we can build a DFA that can keep track of the set of all possible states that the NFA could be in.

The states of the DFA should be the subset of all possible states of the NFA.

### Worked example: epsilion-closures

Given NFA $M = (Q, \Sigma, \delta, q_0, F)$, we want to build DFA $M' = (Q', \Sigma ', \delta ', q_0 ', F')$ (with $L(M) = L(M')$.
- Define $Q= = 2^Q = \\{S : S \sube Q \\}$.
- Define $F' = \\{ S \sube Q : \text{S contains an accepting state of M} \\} = \\{ S \sube Q : S \cap F \neq \emptyset \\}$
- What is the starting state of the DFA?
	- $q_0' = E(\\{q_0\\})$ the set containing $q_0$, or any epsilon arcs of it
- Lastly, $\delta'(S,a)$
	- What is the transition function supposed to do? It's supposed to say, if the DFA is in a certain state, what is the next state the DFA should transition to?
	- From any state in S, we can follow any number of $\epsilon$-transitions, then any transition labelled by $a$, then any number of $\epsilon$-transitions.
	- Write $E(S) = \\{s_0, \ldots, s_k\\}$. What states can I get to from $s_i$ after reading a? $\delta(s_i, a)$
- $\delta'(S,a) = E(\delta(s_1,a) \cup \delta(s_2,a) \cup \ldots \cup \delta(s_k,a)) = E(U_{S \in E(S)} \delta (s,a))$
- What is $\delta'(S,0)$?
	- From S, upon reading a 0 from the input string... can be in states $\\{b, h\\}$.
	- From $\\{b, h\\}$, upon following any $\epsilon$-transitions... can be in states $\\{b,c,h\\}$.

## Regular Expressions

Last time: we showed that regular languages are closed under union, concat, star.

Informally: A **regular expression** is an expression describing a language using only union, concat, star: ($\cup, \circ, \star$).
- Examples: given $\Sigma = \\{0,1\\}$
- Regex: $\Sigma* 1 \Sigma*$:
	- Starts with any string of symbols from Sigma, followed by a 1, followed by any set of symbols from Sigma.
	- $L(r) = \\{ x \in \Sigma^k : \text{x contains a 1} \\}$
- Regex $0*1*$:
	- $L(R) = \{0^i1^j : i \geq 0, j \geq 0 \\}$
- Regex $(0*1*) \cup (1*0*)$
	- $L(R) = \\{ \text{ones followed by zeros, or vice versa} \\}$

Theorem: A language is **regular** if and only if it is described by a regular expression.

Proof has two directions (via $\iff$):
1. $\Leftarrow$: Given a regular expression, find an NFA that recognizes the same language
2. $\Rightarrow$: Given an NFA/DFA, find an equivalent regular expression.

Definition of a regular expression: base cases
- R is a regular expression if R is
	- $a$, for some $a \in \Sigma$
	- $\epsilon$, the empty string
	- $\emptyset$, the empty set

Definition of a regular expression: recursive cases
- $R_1 \cup R_2$ (or $R_1 \mid R_2$), where $R_1$ and $R_2$ are regular expressions
- $R_1 \circ R_2$ (or $(R_1 R_2)$), where $R_1$ and $R_2$ are regular expressions.
- $(R_1 *)$, where $R_1$ is a regular expression

...examples of the above with graphical finite automata...

Standard regex searches pretty much simulate an NFA, and see if any strings match that NFA.

Example: terrible Twitter regex
- Regex can be useful, powerful, but also kind of dangerous (i.e. adversarial inputs)

Regular expressions are great! But there are languages which are not regular.

## Random Walks and Cycles

Random walks in two dimensions
- Property of random walks in 2d: you're very likely to find your way back to the origin at some point

Random walks in three dimensions
- Property of random walks in 3d: you're very _unlikely_ to find your way back to the origin at some point

Concept: when might we be able to find "cycles" in these walks?

Example: Map of a 4km x 4km block of Rio de Janero.
- Suppose I find a "walk" from one corner to the opposite corner that is 100km. Is there an even longer walk?
- Answer: Yes, as you must have hit a cycle: all the streets in that block add up to less than 100km.

Main idea: Suppose we have a directed graph with p vertices. If I make a walk of length > p, then this walk **must contain a cycle**.
- And you can get a longer walk via going around the cycle...

How is this relevant to finite automata?

```automata
--> q1: x->q2
q2: y->q2, z->13
acc q3
```
Input string is XYZ. What can we say about this computation?
- XYYZ is also accepted by the DFA. And XYYYZ, and so forth.

We want to use this property to help us say when a language is non-regular.

Intuition about non-regular languages:
- Finite automata have a **finite amount of state**
- Intuition: A language cannot be regular if it
	- requires comparing lots of things that are far apart
	- involves "counting" something
- Examples:
	- $A = \\{ xx : x \in \Sigma* \\}$ is not regular
	- $B = \\{ 0^n 1^n : n \geq 0 \\}$ is not regular
- Caution: intuition can be wrong!
	- $D = \\{ w \mid w \text{contains an equal number of occurrences of the substrings 01 and 10} \\}$ is regular
	- $B = \\{ 1^k y \mid y \in \\{0,1\\}* \text{and y contains at least k 1s, for} k \geq 1 \\}$ is regular

Question: Let L be a finite language. Is L regular?
- Yes. You _could_ hardcode every possibility into a DFA.

## The Pumping Lemma

Every regular language $L$ satisfies the Pumping Condition.

**The Pumping Condition** for language $L$:
- There exists an integer $p>0$ such that, for every string $w \in L$ with $|w| \geq p$, there exist strings $x,y,z \in \Sigma*$ such that
	- $w = xyz$: w can be composed into xyz
	- $y \neq \epsilon$: y should not be trivial
	- $|xy| \leq p$: can assume xy is short
	- $xy^iz \in L$, for all $i \geq 0$: can **repeat y any number of times** while remaining in L.

...helpful diagram...
- Regular languages $\sub$ All languages satisfying the pumping condition
- Regular languages $\nsub$ All languages not satisfying the pumping condition
- And there exist languages that satisfy the pumping condition but are not regular.
- We can use the pumping condition to show that a language _is_ regular, but not _not_ regular (rephrase).

Note that: All languages not satisfying the pumping condition == all languages satisfying the negation of the pumping condition.

Negating the pumping condition is not exactly easy.
But we can do it 121 style: by pulling the negation through the expression, converting $\sim (\exists : \text{expression})$ to $\forall : \text{expression}$.

**The Negation of the Pumping Condition**
- For all integers $p>0$, there exists a string $w \in L$ with $|w| \geq p$ such that
- for all decompositions $w = xyz$ (i.e. $x,y,z \in \Sigma*$ and $y \neq \epsilon$ and $|xy| \leq p$)
- there exists $i \geq 0$ such that $x y^i z \notin L$.

<!-- Just for fun, entirely in symbols: $(\forall \p \in \mathbb{Z} : p > 0) : (\exists w \in L : |w| \geq p) : (\forall w = xyz, \exists i \geq 0 : x y^i z \notin L)$ -->

<!-- Pumping Lemma Examples
- Theorem: $L = \\{ 0^n 1^n : n \geq 0 \\}$ is not regular.
- Proof:
	- Let $p \geq 1$ be arbitrary.
	- Choose $w = 0^{p/2} 1^{p/2}$ (if p is odd, can use ceil(p/2))
	- $w = 0000000000...011111111111...1$
	- Consider all **decompositions** of $w = xyz$, with $|y| > 0$ and $|xy| \leq p$.
- 3 possibilities:
	- kind of hard to write out, but:
	- x is only 0s, y only 0s, z 0s and 1s
	- x 0s and 1s, y only 1s, z only 1s
	- x only 0s, y 0s and 1s, z only 1s
- If $i=0$: xz has more 1s than 0s: $\implies xz \notin L$. (number of 0s and 1s not equal)
- if $i>1$: $xy^iz$ has more 1s than 0s: $\implies $xy^iz \notin L$.

the above section is pretty poorly written

A smarter proof
- Theorem: $L = \\{ \\}$

-->
