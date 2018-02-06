# Other data structures
## NP classes
### What is NP?
NP is the set of all **decision problems** (questions with a yes-or-no answer) for which the 'yes'-answers can be **verified** in polynomial time (O(n^k) where *n* is the problem size, and *k* is a constant) by a deterministic Turing machine. Polynomial time is sometimes used as the definition of fast or quickly.

### What is P?
P is the set of all decision problems which can be **solved** in *polynomial time* by a *deterministic Turing machine*. Since they can be solved in polynomial time, they can also be verified in polynomial time. Therefore P is a subset of NP.

### What is NP-Complete?
A problem x that is in NP is also in NP-Complete *if and only if every* other problem in NP can be quickly (ie. in polynomial time) transformed into x.

In other words:

1. x is in NP, and
2. Every problem in NP is *reducible* to x

So, what makes NP-Complete so interesting is that if any one of the NP-Complete problems was to be solved quickly, then all NP problems can be solved quickly.

### What is NP-Hard?
NP-Hard are problems that are at least as hard as the hardest problems in NP. Note that NP-Complete problems are also NP-hard. However not all NP-hard problems are NP (or even a decision problem), despite having NP as a prefix. That is the NP in NP-hard does not mean non-deterministic polynomial time. Yes, this is confusing, but its usage is entrenched and unlikely to change.

### Summary:
![NP classes](https://upload.wikimedia.org/wikipedia/commons/a/a0/P_np_np-complete_np-hard.svg)

## Example classes
### Knapsack problem
#### Too easy: splittable items
Greedy (non NP-complete) approach:
*Pick largest value(i)/weight(i) first, and split last item*
#### Non-splittable items (0/1 Knapsack)