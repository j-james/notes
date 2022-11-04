---
layout: default
title: Complexity Theory
---

Do all problems have efficient algorithms?

## Elephant Jokes

- How do you shoot a blue elephant?
	- With a blue elephant gun.
- How do you shoot a red elephant?
	- Hold its trunk shut until it turns blue, then shoot it with a blue elephant gun.
- How do you shoot a purple elephant?
	- Paint it red, hold its trunk shut until it turns blue, then shoot it with a blue elephant gun.

## Reductions

Say we have two problems:
- Problem 1: Shoot a red elephant.
- Problem 2: Shoot a blue elephant.

We can say that $P_1$ **reduces to** $P_2$ if we can design an algorithm for solving $P_1$ that uses $P_2$.
- $P_1 \leq_T P_2$
- $P_1$ reduces to $P_2$
- $P_1$ can be solved in terms of $P_2$
- $P_1$ can be solved using $P_2$ as a subroutine
- IMPORTANT: Note the order! This is easy to flip-flop!

Given our original problems: this means that the problem of shooting a red elephant can be solved by shooting a blue elephant.

What else does this tell us?
- If $P_2$ is easy, then $P_1$ is easy
- If $P_1$ is hard, then $P_2$ is hard (we'll use this idea a lot)
- $P_1$ is **no harder than** $P_2$
- $P_2$ is **at least as hard as** $P_1$

We can similarly talk about languages this way. $L_1 \leq_T L_2$ means:
- deciding $L_1$ is **no harder than** deciding $L_2$, or
- deciding $L_2$ is **at least as hard as** deciding $L_1$.

### Example: Showing the Halting problem to be undecidable

Last time: $A_{TM}$ is undecidable.
- $A_{TM} = \\{\<M,w\> : M \text{ accepts w}\\}$
- $HALT_{TM} = \\{\<M,w\> : M \text{ halts on input w}\\}$

Recall: $HALT_{TM}$ is recognizable
- You can create a Turing machine $U$ that takes an $\<M,w\>$ and simulate it: if $U$ halts, our original Turing machine $M$ accepts

We want to show that $A_{TM} \leq_T HALT_{TM}$. Proof by contradiction:
- Suppose there is a Turing machine $R$ that decides $HALT_{TM}$ (it doesn't exist).
- We want to use $R$ to design a Turing machine $S$ that decides $A_{TM}$. Design:
	- Use $R$ to reject if the input runs forever
	- Otherwise, simply simulate the input (it is now guaranteed to be decidable)
- So we can, and we do, and we arrive at a contradiction: as we know there is no such decider for $A_{TM}$.

Then as $A_{TM}$ reduces to $HALT_{TM}$: $HALT_{TM}$ is at least as hard as $A_{TM}$, which is undecidable. So $HALT_{TM}$ is undecidable.

Aside: reduction can go both ways! $A_{TM}$ reduces to $HALT_{TM}$, and vice versa. There are (probably) problems that cannot be related, however.

## Computability vs Complexity

What is the difference between this theory of computation and Turing machines, vs. complexity?

Computability theory studies whether problems have **any** algorithmic solution whatsoever.

Complexity theory studies whether problems have **efficient** algorithmic solutions.
- How much time is required?
- How much space is required?
- Does randomness help?

### Worst case analysis

Usually theorists do "worst case analysis", i.e. as a function of the input length $n$, what is the maximum amount of resources used over all inputs of length $n$? (note: $n$ is a commonly used variable!)

The idea here is that it's helpful to analyze the worst case, as every input will then result in equal or better time.

Definition: The **runtime** of a Turing machine $M$ is $f : \mathbb{N} \to \mathbb{N}$: $f(n) = max(x \in \Sigma\*, \|x\|=n)$. (the number of steps of $M$ on input $x$ until it halts) (assume $M$ is a decider for simplicity)

Definition: a **class** is a set of languages (or computational problems)

Definition: a **complexity class** is a class defined by Turing machine resource constraints.

Definition: **TIME(t(n))** is the class of all languages $L$ such that there exists a Turing machine with runtime $O(t(n))$ that decides $L$.

Recall: different machine models have different efficiencies: our multi-tape Turing machines were broadly more efficient than our single-tape Turing machines. For example:
- $PAL = \\{ww^R : w \in \Sigma\*\\}$
- Decided by a single-tape Turing machine in $O(n^2)$ time
- Decided by a multi-tape Turing machine in $O(n)$ time

## Revisiting the Church-Turing thesis

The **Extended Church-Turing thesis** is the belief that Turing machines capture our intuitive notation of what is **efficiently computable**.
- Everything we can compute in time $t(n)$ on any physical computer can be computed on a Turing machine in time $O(t(n)^c)$, for some constant $c$.

Is this true, does it hold? Maybe not, with new forms of computation like quantum computers.

## P and EXP

Definition: $P = \large \cup_{c>0} TIME(n^c)$

Why is P a good definition for efficient computation?
- Insensitive to model of computation: by the extended Church-Turing thesis, switching computational models "only" involves a polynomial slowdown
- Closed under composition: can use polynomial time algorithm as a subroutine
- In practice: **algorithms usually can be improved**. If we find an $O(n^{30})$ time algorithm, it typically can be improved to, say, $O(n^5)$ time.

### Example: Two color map

Let $G$ be a "map" of countries in the plane, where a coloring is valid if neighboring countries have different colors.

The two color map _does not_ work for every map. So consider it as a problem: $2COLORMAP = \\{\<G\>: G \text{ has valid red/blue coloring}\\}$. Is $2COLORMAP \in P$?

Sure: start with an arbitrary country, set it to red, set their neighbors to blue, and continue. If there is a conflict, reject, if there is no conflict, accept.

It's unknown if the three color map is in P. It's known to be in EXP: $O(3^n n^2) \in O(4^n)$

Definition: $EXP = \large \cup_{k>0} TIME(2^{n^k})$

The four color map works for all maps, so $4COLORMAP = \\{\<G\> : G \text{ is a valid map}\\}$.

## Time Hierarchy Theorem

Question: is $EXP = P$? Answer: No!

Definition: the **time hierarchy theorem** tells us that given an $f : \mathbb{N} \to \mathbb{N}$ that is "reasonable" where $f(n) = \Omega(n \log n)$, then the $TIME(f(n)) \subset TIME(f(n)^4)$.
- Notably: $TIME(f(n)) \neq TIME(f(n)^4)$!
- Corollary: $TIME(n^c) \neq TIME(n^{4c})$ for any $c \geq 1$.
- Corollary: $TIME(n^c) \neq TIME(2^n)$ for any $c \geq 1$.
- $TIME(2^{n^k}) \neq TIME$

How do you prove the time hierarchy theorem?
- By diagonalization: look at Turing machines that are "fast" and that are "slow"
- Idea: look at all Turing machines that run in "fast" time: and come up with a language that they cannot possibly decide
- But we want that language to be decidable by a "slow" Turing machine.

Language that is in EXP and not in P: ???

todo: rework this entire section, it doesn't make sense

## Time Complexity

Where is 3COLORMAP? We know it is in EXP, and either in NP or in both NP and P.

The Time Hierarchy theorem tells us that there is something in $EXP \setminus P$. [why? still not clear]

![Complexity Hierarchy](../assets/complexity-hierarchy.png)

## NP and Time Complexity

NP is very important! Contains a large amount of natural problems
- Ex. Hamilton path, 3 color graph, graph isomorphism, satisfiable boolean formulas
- Many problems in NP have "identical complexity", i.e. are complete (we'll talk about this later)

Definition: the **runtime** of NTM on input X is the max-length of any root-leaf path
- For deciders - no infinite paths!

Recall: $TIME(t(n)) = \\{\text{language L} : \exists \text{ TM M that decides L in time O(t(n))}\\}$
- $P = \cup_{c>0} TIME(n^c)$

Definition: $NTIME(t(n)) = \\{\text{language L} : \exists \text{ NTM M that decides L in time O(t(n))}\\}$
- $NP = \cup_{c>0} NTIME(n^c)$

Obvious: $P \sube NP$.

<!-- aside: "P is a 'larger class' than NP": but not strictly larger? god, i love unintuitive definitions -->

### Example: 3COLORMAP

Let's prove that $3COLORMAP \in NP$.

Proof: Design a nondeterministic Turing machine to decide 3COLORMAP in polynomial time.
- Iterate through vertices
- Nondetermistically pick a color for each vertex (a "lucky guess")
- Check all pairs of neighbors to see if they are the same color
- If so reject, else accept

### NP and EXP

Claim: $NP \sube EXP$.
Idea: For any $L \in NP$, there is an NTM deciding $L$ in polynomial time. We can simulate it with a DTM that does the breadth-first search on the configuration tree. The tree has a number of nodes _exponential_ in runtime of the NTM.

There are several open questions surrounding P, NP, and EXP:
1. Is $P \neq NP$, i.e. $P \subset NP$?
2. Is $NP \neq EXP$, i.e. $NP \subset EXP$?

The answer to **either Q1 or Q2** is yes. Proof: $P \sube NP \sube EXP$, yet by the time hierarchy theorem $P \neq EXP$. So either $P \neq NP$ or $NP \neq EXP$ (or both).

## Certificates and verifiers

Theorem: NP has two main, equivalent definitions:
- There exists a **nondeterministic**, polytime TM M that decides L.
- There exists a **deterministic**, polytime TM V and a constant c s.t.
	- $L = \\{x \in \Sigma\* : \exists y \in \Sigma\* \text{ s.t. } \|y\| \leq \|x\|^c \land V \text{ accepts } (x,y)\\}$
	- x: "input", y: "certificate", V: "verifier"
	- i.e. for the three color graph, give us a colored graph and we can verify it

Main idea: the certificate y serves as V's substitute for nondeterminism.

### Example: 3COLORMAP

Claim: $3COLORMAP \in NP$.
- $\\{\<G\> : \text{ G is 3-colorable}\\}$
- Must find a deterministic polytime TM V s.t. $3COLOR = \\{x: \exists y, \|y\| \leq \|x\|^c, V \text{ accepts } (x,y)\\}$
- Code for $V$: on input (x,y)
	- If $x$ not of form $\<G\>$, where $G=(V,E)$ is a graph, reject
	- From $y$, extract values $color[v] \in \\{1,2,3\\}$ for each $v \in V$
	- For every edge $\\{u,v\\} \in E$: If $color[u] = color[v]$, reject
	- Otherwise accept
- $y$ does exist, it's simply a valid coloring of the graph

To show correctness of V, we need three things:

1: If $x = \<G\>$ and G is 3-colorable: need $\exists y \text{ s.t. } \|y\| \leq \|x\|^c$ and $\text{V accepts (x,y)}$.
- It does exist, y can be a valid 3 coloring of G. Note: $\|y\| = O(n)$ and $\|x\| = \Omega(n)$

2: If $x \neq \<G\>$ where G is 3-colorable: then no certificate y should make V erroreously accept.
- If x not of form $\<G\>$: v definitely rejects.
- If x of form $\<G\>$ by not 3-colorable, then V must reject becasuse it always checks that y is a valid coloring.

3: Must check that V runs in time polynomial in |xy|.
- Decoding takes polytime, checking neighbors takes polytime.

<details markdown="block">
<summary markdown="span">Proofs of NP definition</summary>

Proof: $(2) \implies (1)$
- Given verifier V, must construct a NTM M deciding L
- Inputs to the verifier are x and y, with $\|y\| \leq \|x\|^c$
	- x: input string
	- y: certificate (like a proof that x is in the language)
- Code for M:
	- On input x, nondeterministically pick $y \in \Sigma\*$ with $\|y\| \leq \|x\|^c$
	- Run V on input (x,y)
	- Accept if V accepts. Reject if V rejects.
i.e. Suppose $x \in L \implies \exists y$ s.t. V accepts (x,y) $\implies$ there is a leaf where M accepts.
If $x \notin L \implies \notexists y$ s.t. V accepts (x,y) $\implies \notexists$ leaf, so M does not accept x.

Proof: $(1) \implies (2)$
- Given an NTM M, you can convert that into a verifier $V$ for $L$.
- Input to $M$ is $x$, runtime is at most $\|x\|^c$
- **Idea**: $V$ simulates the tree, but _only_ on a single branch of configuration tree. $y$ specifies which branch. nondeterminism comes into play by letting us pick the correct branch.
	- our nondetermism lets us pick the correct branch: then the $c$ term is the depth of the path, which is at most polynomial
- Code for M:
	- Simulate M deterministically on input x, using y as the sequence of nondeterministic decisions
	- Accept if M accepts. Reject if M rejects.

</details>

## Why NP is nice

We'll spend the next couple of lectures talking about NP. Why? Nondeterministic Turing machines are ridiculously impractical!

Many problems that we're interested in have the form $L = \\{ x : \exists y, |y| \leq |x|^c, V \text{ accepts } (x,y) \\}$
- x: problem input
- y: object we are seeking
- efficient test: is y a valid solution?

We showed $3COLOR \in NP$.
- Claim: $CLIQUE \in NP$ ($CLIQUE = \\{\<G,K\> : \text{ G contians a clique of size k }\\}$)
	- (a clique is a subgraph that has all possible edges)
- Claim: $HAMPATH \in NP$ (Hamiltonian path problem)

NP has lots of problems that are interesting and nice to solve, but that we don't know of any polynomial time solution. But: if _any_ of these problems are in P, then _all_ of them are in P. NP-completeness!

## The SAT and 3SAT problems

The canonical problem in NP (like how $A_{TM}$ was our canonical undecidable problem)

$SAT = \\{\<\phi\> : \phi \text{ is a Boolean formula for which there exists a satisfying truth assignment} \\}$
- e.g. $\phi(x_1, x_2, x_3, x_4, x_5) = (((x_1 \lor x_2) \land (x_1 \lor \lnot x_3 \lor x_4)) \lor \lnot (x_2 \land x_3) \lor x_4) \land \lnot x_5$
	- helpful to visualize with a tree diagram

A satisying truth assignment is an assignment of TRUE/FALSE to each variable so that the whole formula is TRUE.

Definition: A problem is in **conjunctive normal form** if it is a product of sums: i.e. an AND of ORs.

$3SAT = \\{\<\phi\>: \phi \text{ is a Boolean formula in CNF with 3 literals per clause for which there exists a satisfying truth assignment}\\}$
- e.g. $(x_1 \lor \lnot x_2 \lor x_3) \land (x_5 \lor x_4 \lor \lnot x_2) \land (\lnot x_4 \lor x_1 \lor x_3) \land (\lnot x_3 \lor x_2 \land \lnot x_1)$

## Polytime Reduction

Definition: For some function $f : \Sigma\* \to \Sigma\*$, it is **polytime computable** if there exists a Turing machine that when given $x$ as input terminates with $f(x)$ on the tape and runs in time $poly(\|x\|)$.

Definition: Let $A, B \sube \Sigma\*$ (Let A and B be languages). $f$ is a **polytime reduction** from $A$ to $B$ if:
- $f$ is a polytime computable function
- $x \in A \implies f(x) \in B$ ($f(A) \sube B$)
- $x \notin A \implies f(x) \notin B$ ($f(A^{\_} \sube B^{\_}$)
We then may say $A \leq_P B$.

<!-- ...helpful image... -->

### Example

Suppose $A = \\{0^n 1 0^n : n \geq 0\\}$, and $B = \\{\1^n 0 1^n : n \geq 0\\}$. Claim: $A \leq_P B$

What does this mean? It means there exists some function that can take a string, map it, and manipulate it so that if $x \in A$, then $x \in B$, and same for converses.
- Proof: Let $f$ be the function that flips bits of its input. Then, done.
- $x \in A \implies f(x) \in B$
- $x \notin A \implies f(x) \notin B$
- $f(x) \in B \implies x \in A$
- $x \in A \iff f(x) \in B$
- Note: these **do not need to be bijections!** A reduction that simply sends all satisfiable inputs to 0 is fine.

### Facets of reductions

Reductions are helpful!
- Method: find a way to map:
	- yes-instances of $P_1$ to yes-instances of $P_2$
	- no-instances of $P_1$ to no-instances of $P_2$
- Notation: $P_1 \leq_P P_2$
- Logic: if $P_2$ is easy, $P_1$ is easy
- Contrapositive: if $P_1$ is hard, $P_2$ is hard

Theorem: If $A \leq_P B$ and $B \in P$, then $A \in P$.
- There is a polytime Turing machine N that decides B.
- There is a polytime reduction f from A to B.
- Proof: We want a Turing machine M that decides A in polynomial time.
	- On input x: compute z = f(x) and then simulate N on input z. Accept iff N accepts.
	- If $x \in A \implies f(x) \in B \implies \text{ N accepts}$.
	- If $x \in A^_ \implies f(x) \notin B \implies \text{ N rejects}$.

Question: if we know A reduces to B and B is in NP, is A in NP?
Answer: Yes, P is a subset of NP. Mind the direction!

When we talked about Turing machines, we had a notion of what it meant to be hard: undecidability. But now we're talking about NP. So which problems in NP are hard?

Maybe $P=NP$, and so all problems in NP are easy.

## NP-Hardness

Definition: A language L is **NP-hard** if it is _at least as hard_ as every problem in NP, i.e. $A \leq_P L, \forall A \in NP$.
- There's nothing in this definition that's really specific for NP. We could switch it out for other complexity classes if we'd like.
- Note: we don't require our language to be in NP! $A_{TM}$ is NP-hard.

Definition: A language L is **NP-complete** if L is NP-hard and $L \in NP$.

Definition: The **Cook-Levin Theorem** says that SAT is NP-complete. Corollary: 3SAT is also NP-complete.

Theorem: If B is NP-complete, $C \in NP$, and $B \leq_P C$, then $C$ is NP-complete.
- Need to show: $A \leq_P C, \forall A \in NP$
- We know $A \leq_P B$, i.e. you can solve A in terms of B.
	- So $\exists$ a polytime computable $f : x \in A \iff f(x) \in B$.
- We know $B \leq_P C$: $\exists$ a polytime computable $g : y \in B \iff g(y) \in C$
- Let $h : \Sigma\* \to \Sigma\*$ be $h(x) = g(f(x))$. This is polytime computable.
	- Has the properties: $x \in A \iff f(x) \in B \iff g(f(x) \in C$.

When designing a reduction: if you want to show that your reduction is correct: you need to show that
- f is polytime computable
- $x \in A \implies f(x) \in B$
- $f(x) \in B \implies x \in A$ (the converse)
- or, $x \notin A \implies f(x) \notin B$ (the inverse)

In a reduction, it's more typical to show $x \in A \implies f(x) \in B$ (often straightforward).
- Often want to "decode" $f(x) \in B$ to obtain $x \in A$.

### Example: CLIQUE problem

![Simple Clique](../assets/simple-clique.png)

Given $G=(V,E)$ and $k$, does there exist $U \sube V$ with $|U| \geq k$ such that $\\{u,v\\} \in E$ for all distinct vertices $u,v \in U$?

$CLIQUE = \\{\<G,K\> : \text{ G has a clique of size k}\\}$
- No specification on size: in our example above, there are cliques of size 0 through 3

Theorem: CLIQUE is NP-complete: i.e. CLIQUE is NP-hard, and CLIQUE is in NP.
- CLIQUE is in NP: because there exists a verifier that can check a possible clique in polytime.
- CLIQUE is NP-hard: because 3SAT (known NP-hard problem) reduces to CLIQUE. How can we do that?

Reduction $3SAT \leq_P CLIQUE$
- $x \in 3SAT$ maps to $f(x) \in CLIQUE$
	- If $x=\<\phi\>$ where $\phi$ is a satisfiable 3CNF formula, then $f(x) = \<G,k\>$ where $G$ has a clique of size $k$.
- $x \notin 3SAT$ maps to $f(x) \notin CLIQUE$
	- If $x=\<\phi\>$ where $\phi$ has no satisfiable assignments, then $f(x) = \<G,k\>$ where $G$ has no clique of size $k$.
	- junk can map to junk

![Complex Clique](../assets/complex-clique.png)

We can show $3SAT$ can be solved by finding a clique of size $k$.
- Connect every vertex in a block to every other vertex that is **not** its inverse.
- Then, every clique of size $k$ is a **possible satisfiable assignment**.
- We can't have $x_i$ and $x_i^{\_}$ both being true: which is represented by no connecting edges.

We still need to "decode" $f(x) \in B$ to $x \in A$... i.e. $f(x) \in CLIQUE \implies x \in 3SAT$.
- Let $U$ be a clique of size $k$. We must divy it up so that there is exactly one vertex in each group.
- Given a $f(x) \in \text{ CLIQUE}$ constructed from a 3SAT problem, then we find a satisfying $x \in \text{ 3SAT}$.

Aside: suppose we want to ensure there is a clique of at least size $k-1$. We can make a separate, disconnected clique of $k-1$ vertices. What's this useful for? Don't know yet...
