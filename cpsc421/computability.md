---
layout: default
title: Computability Theory
---

Can all software be verified?

## Computational Problems
1. Given an integer n, determine if n is prime.
- For every integer k between 0 and n, check if k divides n.
2. Given an integer n, determine the nth digit of $\pi$.
- Use an infinite series.
3. Given a polynomial in n variables with integer coefficients, determine if it has an integer root.
- Hilbert's 10th problem, cannot be solved by any algorithm
	- Proven by Davis, Matyiasevich, Robinson

Hilbert in 1928: The "Entscheidungsproblem"
- Means "Decision Problem"
- Find an algorithm that given any logical statement, decides if it can be proven
Solutions: Church (1936), Turing (1937)
	1. Give a formal definition of an algorithm
	- Church: The Lambda-Calculus
	- Turing: Turing Machines
2. Show that there is no algorithm for the decision problem
- Fairly straightforward from ideas at the time, i.e. Godel's incompleteness theorem

## Aside: Wang Tiling Problem

![13 Wang tiles](https://upload.wikimedia.org/wikipedia/commons/f/f3/Wang_tiles.svg)

Question: Do these tiles tile the infinite plane?
- Answer: Surprisingly, yes, and more surprisingly, _aperiodically._

As it turns out: any Turing machine can be "simulated" using a Wang tiling.
- Given any algorithm for a decision problem, you can find a set of tiles that tile the plane iff the algorithm outputs "yes" (or runs forever)
- In other words: Wang tilings are Turing complete.

After some time: The smallest set of Wang tiles that are Turing-complete are 11 tiles with 4 colors.

![11 Wang tiles](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Wang_11_tiles.svg/316px-Wang_11_tiles.svg.png)

## Turing Machines

**Church-Turing Thesis** (informally): Our intuitive notion of algorithm is equivalent to a Turing machine.

"Turing complete" means "as powerful as Turing machines".

Turing machines can be implemented in some weird ways:
- Molecules
- Carbon nanotubes
- Wang tilings
- Any product by Microsoft

What are Turing Machines, anyway? They require:
- An _infinite_ tape
- A finite state controller to make decisions, control tape, etc
- Turing machines can write to the tape, read from the tape, move the tape
- Input string initially at the start of the tape

Formal definition of a **Turing machine**
- Definition: A Turing machine is a 7-tuple $T = (Q,\Sigma,\Gamma,\delta,q_0,q_{accept},q_{reject})$ where
- $Q$ is the set of **states**; non-empty and finite
- $\Sigma$ is our **alphabet**; non-empty and finite (without the blank string, $\\_ \notin \Sigma$)
- $\Gamma$ is our **tape alphabet**; non-empty and finite
- $\delta : Q \times \Gamma \rightarrow Q \times \Gamma \times \\{L,R\\}$ is the **transition function**
- $q_0 \in Q$ is the **start state**
- $q_{accept}, q_{reject} \in Q$ are the **accept and reject states**
Note: $\Sigma \sube \Gamma$ and the blank symbol $\in \Gamma \backslash \Sigma$

Key difference between Turing machines and our previous models of computation: Turing machines can run forever
- Not possible to have an infinite loop with a DFA or (kinda) a PDA: you have a finite string that you are proceeding through, so you cannot loop.
- This is why we need an explicit $q_{accept}$ and $q_{reject}$!

### Recognizable vs. Decidable
- The **language recognized by** a Turing machine M is $L(M) = \\{x : \text{ M accepts on input x } \\}$
- If M halts on all inputs, M is called a **decider**.
- If M is a decider then $L(M)$ is also called the **language decided by** M.
- A language is **recognizable** if some Turing machine recognizes it.
- A language is **decidable** if some Turing machine decides it.

![Venn diagram](https://i.stack.imgur.com/CJc44.png)
- Regular Languages $\sube$ Context-Free Languages
- Context-Free Languages $\sube$ Decidable Languages
- Decidable Languages $\sube$ Recognizable Languages

## Aside: Conway's FRACTRAN

What does the following algorithm do, given these fractions as input?

```nim
import std/math

let fractions = @[
	(17,91), (78,85), (19,51), (23,38), (29,33), (77,29), (95,23), 
	(77,19), (1,17),  (11,13), (13,11), (15,14), (15,2),  (55,1)
]

var x = 2
while true:
  if x.isPowerOfTwo:
    echo log2(x.float)
  for frac in fractions:
    if x mod frac[1] == 0:
      x = (x div frac[1]) * frac[0]
      echo x
      break
```

Answer: this program computes primes.

This _algorithm_ is Turing-complete, acting as a form of a register machine: different fractions can be provided to make it compute anything (anything computable).

## Configuration of a Turing Machine

Definition: The **configuration of a Turing machine** is a string $aqb$ where $a \in \Gamma\*$, $q \in Q$, $b \in \Gamma\*$ (assume $Q \cap \Gamma = \emptyset$).
- The Turing machine is in state $q$
- The tape content is $ab$ (ignore blanks at end)
- The head is on the first symbol of $b$
- Initial configuration: $q_0 x_{input\ string}$
- Accepting configuration: $a q_{accept} b$ for any $a,b \in \Gamma\*$
- Rejecting configuration: $a q_{reject} b$ for any $a,b \in \Gamma\*$

How does the transition function affect the config?
- $\delta(q_0,g) = (q_j,h,L)$
- Config: $uf q_i gv$ -> $u q_j fhv$

General idea: if you have a configuration, then the configuration + transition function describes the Turing machine.
- There's also only three characters that can change: under the tapehead, to the left of the tapehead, and to the right of the tapehead.

Behavior of deterministic Turing machines
- Start = Config(1) = ...
- Config(2) = ...
- Config(i).transition() determines Config(i+1)
- **Acceptance**: does config ever have $q_{accept}$?
- **Rejection**: does config ever have $q_{reject}$?

## Multitape Turing Machines

A useful variant: $k$-tape Turing machines

Informal definition of a **multitape Turing machine**:
- Input is written on the left-most squares of Tape I
- Rest of squares are blank, on all tapes
- At each point, step based on current state and current symbols
- A step is:
	- writing or not writing symbols on each tape
	- moving each head left/right
	- changing state

Formal definition of a **multitape Turing machine**:
- Same as a regular Turing machine, only the transition function changes: to read/write/move on $k$ tapes
- $\delta : Q \times \Gamma^k \rightarrow Q \times \Gamma^k \times \\{L,R\\}^k$

Theorem: Every multitape Turing machine has an equivalent single-tape Turing machine. We can show this by describing how to simulate a multitape Turing machine on a single-tape Turing machine: simply interleave the tapes, and jump by $k$ characters instead of jumping by 1 character each time.

Takeaway: multitape Turing machines are no more powerful than single-tape Turing machines: but they can be _faster_.

## Nondeterministic Turing Machines

Definition of a **nondeterministic Turing machine**:
- Same as a regular Turing machine, only the transition function changes: $\delta$ now outputs _sets_ of triples.
- $\delta : Q \times \Gamma \to 2^{Q \times \Gamma \times \\{L,R\\}}$
We also need to change the definition of acceptance: if there's any sequence of transitions leading to $q_{accept}$, then it accepts.
- Formalize this using a configuration tree (as opposed to a sequence).
- For any node in the tree with configuration C, add children $C'$ for all configurations that could follow from C.
- Children are all the configurations that a transition function can yield, leaves are accept/reject configs.
- The NTM accepts its input if any leaf in the tree is an accepting configuration.

What might a tree look like if there's no accepting node in the tree?
- Could still have reject leafs...
- Could have some infinite branches...

What does it mean for a nondeterministic Turing machine to be a **decider**?
- We'll say that it's a decider if for all inputs, **the configuration tree is finite**.

Important asymmetry:
- to accept input, only a single config leaf must accept.
- to reject input, **all leaves** must reject.
- (third case: not a decider)

Theorem (3.16): For every nondeterministic Turing machine $M$ there is a deterministic Turing machine $M'$ with $L(M) = L(M')$, i.e. nondeterministic Turing machines can be simulated by deterministic Turing machines.

In addition, if $M$ is a decider, than we can also assume that $M'$ is a decider.

### Simulating NTMs with DTMs

Main idea: breadth-first search of tree
- If $M$ accepts: we will encounter an accepting leaf and accept
- If $M$ rejects: we will encounter all rejecting leaves, finish traversal of tree, and reject
- If $M$ does not halt on some branch: we will not halt...
Note that a depth-first search will fail with our infinite tree!

Given $M$ and given $x$, I want $M'$ to simulate $M$ on input $x$.

### Simulating NTM deciders with DTM deciders

... missed some stuff ...

Main takeaway: nondetermistic Turing machines are no more powerful than deterministic Turing machines, deciders or not!

- Recall: definition of the deterministic decider (halts on all inputs)
- Recall: definition of the nondeterministic decider (tree is finite)

If we have a nondeterministic Turing machine:
- If the tree is infinite, the simulation will run forever.
- If the tree is finite, we can tweak the simulator so that it always terminates.

Theorem: For every nondeterministic Turing machine decider, there is an equivalent deterministic Turing machine decider.

## Encodings

So far, all of the inputs to our Turing machines have been strings: which aren't terribly interesting.

Idea: but everything's just bits! Take more complicated data structures and convert them to strings (a sequence of bits). Examples of such: JSON, Python pickles, code

For any object, say graph G, we'll put angle brackets around it: $\<G\>$ to mean G converted into a string format. We can serialize a graph, DFA, Turing machine, etc with this approach.

Example: $L = \\{\<G\> : G is a converted graph\\}$
- Accept: valid encodings of connected graphs
- Reject: valid encodings of disconnected graphs
- Reject: invalid encodings
A Turing machine that recognizes L must detect invalid encodings and reject them.

We can also look at strings that are encodings of Turing machines.

Recall: $HALT_{TM}$ and $A_{TM}$
- $HALT_{TM} = \\{\<M,w\>: \text{ M is a TM that halts on input w}\\}$
- $A_{TM} = \\{\<M,w\>: \text{ M is a TM that accepts input w}\\}$

Neither are decidable languages. However: both are recognizable languages.

## Universal Turing Machine

Inputs:
- Encoding of a Turing machine $\<M\>$
- Some string $w$
Outputs:
- ACCEPT if and only if $M$ accepts $w$

A humorous example, in Python:
```python
import sys
M = sys.argv[1]
w = sys.argv[2]
exec(M)
```

Example: A Turing machine U that recognizes $A_{TM}$
On input X:
- If $x = \<M,w\>$ where $M$ is a Turing machine and $w \in Sigma\*$:
	- Simulate $M$ on input $w$
	- If $M$ enters accept state: ACCEPT
	- If $M$ enters reject state: REJECT
- Else, REJECT

Behavior:

$M$ on input $w$ | $U$ on input $\<M,w\>$
-----------------|-----------------------
accepts          | accepts
rejects          | rejects
loops            | loops

Conclusion: U accepts $\<M,w\>$ $\iff$ M accepts w. So U recognizes $A_{TM}$.

## Countable vs. Uncountable Sets

Definition: Two sets A and B have the same **cardinality** if there is a bijection (both injective and surjective, one-to-one and onto) $f$ mapping $A$ to $B$ (or $B$ to $A$).

- Definition: A set is **countably infinite** if it has the same cardinality as the natural numbers.
- Definition: A set is **countable** if it is finite or countably infinite.
- Definition: A set is **uncountable** if it is infinite but not countable.

Theorem: Every language $L$ is countable.
Proof: Every $\Sigma\*$ is countable, so $L$ must be countable.
Claim: If we have an injective map $f: A \to \mathbb{N}$ then $A$ is countable.
Proof: If A is finite, done. If f is surjective, done. Otherwise $image(f) \subset \mathbb{N}$, but we can enumerate $image(f)$ as $\\{b_1,b_2,b_3,\ldots \\}$. Now we have a bijection from $image(f)$ to $\mathbb{N}$.
- $image(f)$: the set of all outputs of the function $f$

Theorem: The set of all Turing machines is countable.
Proof: Every Turing machine can be described by a string $\<M\>$. Let $L$ be the language of all encodings of Turing machines. This is a language, hence it is countable.

Theorem: The positive rational numbers $\mathbb{Q} = \\{\frac{m}{n} : m, n \in \mathbb{N}\\}$ are countable.
Proof: Consider ordering the rational numbers in a grid, and going through them diagonally, while skipping duplicates. That provides an ordering of rational numbers: which can then be described by an injection.

## The Halting Problem

General idea: there are certain statements that will always result in logical contradictions when attempting to answer them.

Recall: $A_{TM} = \\{\<M,w\>\\ : \text{ M is a TM that accepts w}}$
Last time: we showed that $A_{TM}$ is recognizable.
This time: we'll try to show that $A_{TM}$ is (not) decidable.

Idea: self-referential sentences $\implies$ contradictions, so do self-referential Turing machines $\implies$ contradictions?

Suppose there exists a Turing machine $H$ that decides $A_{TM}$.
Consider the Turing machine $\<M\>$: which outputs 0 if $M$ accepts $\<M\>$, and 1 otherwise. Also consider the Turing machine NEG: which outputs 1 if $M$ rejects $\<M\>$ and 0 otherwise.

What happens when we feed NEG to itself as input? Contradiction!

NEG rejects <NEG> $\iff$ NEG accepts <NEG>

Aside: we spent several lectures building up the idea of Turing machines, only to prove that the Halting problem is undecidable in around 10 minutes.

## The Diagonalization Argument

The idea is due to Georg Cantor.
- So revolutionary that his peers couldn't handle it!
- Poincaré: "a grave disease"; Kronecker: "a scientific charlatan"

Let $\mathbb{B}$ be the set of all infinite bit-sequences.

Theorem: The set $\mathbb{B}$ is **not countable**.
Approach: Proof by contradiction. Proof:
- Assume the set is countable. So there exists a bijection $f : \mathbb{N} \to \mathbb{B}$.
- List the sequences in $\mathbb{B}$, ordered using the bijection $f$ (0-$n$).
- This list contains **every** infinite sequence, because $f$ is a bijection.
- Focus on the diagonal: Let $a_n = n^{th}$ bit of $f(n)$.
- Flip the bits: Let $b_n$ = the opposite of $a_n$
- Assemble the flipped bits: Let $x = b_1 b_2 b_3 b_4 \ldots$
- Observation: $x \neq f(n)$ for any $n$: because $x$ **must** differ from $f(n)$ in the $n^{th}$ digit. Thus $x$ is not on the list.
- Contradiction: The list should contain every infinite bit sequence.
- Conclusion: $f$ does not exist, i.e. $\mathbb{B}$ is not countable.

Why is $\mathbb{B}$ uncountable but $\Sigma\*$ is countable?
- Every element of $\Sigma\*$ is a finite string, while every element of $\mathbb{B}$ is an infinite string.

Corollary: There are uncountably many languages.
Proof:
- Can enumerate all strings in $\Sigma\*$
- Each language $L$ contains some subset of them
- There is a bijection between languages and infinite bit-sequences
- So then the set of all languages is uncountably infinite.
- Conclusion: the class of all languages, $2^{\Sigma\*}$, has the same cardinality as $\mathbb{B}$ (uncountably infinite)

Claim: The set of recognizable languages is countable.
Proof: For every recognizable language L, there is a Turing machine that recognizes it: so there is an injection from recognizable languaes to $L^{TM}$.
