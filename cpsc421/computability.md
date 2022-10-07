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
- Answer: Surprisingly, yes, but _aperiodically._

As it turns out: any Turing machine can be "simulated" using a Wang tiling.
- Given any algorithm for a decision problem, you can find a set of tiles that tile the plane iff the algorithm outputs "yes" (or runs forever)
- In other words: Wang tilings are Turing complete.

After some time: The smallest set of Wang tiles that are Turing-complete are 11 tiles with 4 colors.

![11 Wang tiles](https://upload.wikimedia.org/wikipedia/commons/a/a4/Wang_11_tiles.svg)

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
- $\Sigma$ is our **alphabet**; non-empty and finite (without the blank string, $\textvisiblespace \notin \Sigma$)
- $\Gamma$ is our **tape alphabet**; non-empty and finite
- $\delta : Q \times \Gamma \rightarrow Q \times \Gamma \times \\{L,R\\}$ is the **transition function**
- $q_0 \in Q$ is the **start state**
- $q_{accept}, q_{reject} \in Q$ are the **accept and reject states**
Note: $\Sigma \sube \Gamma$ and the blank symbol $\in \Gamma \slash \Sigma$

Key difference between Turing machines and our previous models of computation: Turing machines can run forever
- Not possible to have an infinite loop with a DFA or (kinda) a PDA: you have a finite string that you are proceeding through, so you cannot loop.
- This is why we need an explicit $q_{accept}$ and $q_{reject}$!

### Recognizable vs. Decidable
- The **language recognized by** a Turing machine M is $L(M) = \\{x : \text{ M accepts on input x } \\}$
- If M halts on all inputs, M is called a **decider**.
- If M is a decider then $L(M)$ is also called the **language decided by** M.
- A language is **recognizable** if some Turing machine recognizes it.
- A language is **decidable** if some Turing machine decides it.
