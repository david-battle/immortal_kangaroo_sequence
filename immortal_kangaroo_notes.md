# The Immortal Kangaroo Sequence — Notes

Companion notes to the Numberphile video "The Immortal Kangaroo Sequence"
(Sep 4, 2026, Brady Haran with Neil Sloane). Cleaned transcript:
`immortal_kangaroo_no_timestamp.txt`. Deep-dive on the ancestor math:
`ancestor_note.md`.

## 1. The comma sequence (Eric Angelini)

The comma sequence starts at 1. Consecutive terms `n` and `m` obey the rule

```
m - n = s
```

where `s` is the *comma separator*: the two-digit number formed by the last
digit of `n` and the first digit of `m`. When there is more than one valid `m`,
take the smallest; when there is none, the sequence terminates.

Example: 1, 12, 35, 94, 135, 186, ... (differences 11, 23, 59, 41, 51, ...).

Properties:
- Jumps are always between 1 and 99, so the sequence only ever takes small
  steps (the "kangaroo" hopping along the number line).
- The process is *memoryless*: what follows a term depends only on that term,
  not on how the sequence got there.
- Every number has 0, 1, or 2 comma-children; never more. With the "firstborn"
  rule (always smallest) there are 0 or 1 successors.

## 2. Landmines (death)

A *landmine* is a number with no successor. In base 10 the landmines are
exactly the numbers whose last two digits sum to 9 (18, 27, ..., 81) with any
number of 9s in front, including zero 9s: `99...9xy` with `x + y = 9`.

- Eight landmines per power-of-10 block, all clustered in the last ~100 numbers
  before each power of 10.
- Starting at 1, the sequence dies after 2,137,453 steps, ending at 99999945.
- In base 10 **every** start eventually dies (computer-aided proof, see §7).
  Conjectured for all bases > 2. Base 2 is the exception: every sequence is
  infinite.
- The expected lifetime for a random start is roughly 10^8.3.

## 3. The ancestor is unique (and computable)

Every number has exactly one parent. Given `m`:

```
b = first digit of m
a = (m - b) mod 10
n = m - (10a + b)
```

`a` is forced because the last digit of `n = m - (10a + b)` must equal
`(m - b) mod 10`. Existence fails exactly when `n < 1`. See `ancestor_note.md`
for the full argument, the fork subtlety (a *parent* can have two children but
a *child* has exactly one parent), and the 50 roots.

## 4. Immortality: existence is non-constructive

- There are two graphs: the successor graph `Gs` (firstborn rule, out-degree
  0 or 1) and the child graph `Gc` (allows either child at a fork, out-degree
  up to 2).
- Every integer traces back to one of **50 roots** in 1..99 (numbers with no
  parent), giving 50 trees.
- All 49 trees other than the one rooted at **20** are finite. Since the whole
  graph is infinite, the 20-tree must be infinite (infinite pigeonhole).
- König's Infinity Lemma then guarantees an infinite branch in that tree.
  This is non-constructive: it needs ZF + AC and gives no way to find the
  path. So we know an immortal path exists but cannot effectively compute it.

## 5. Uniqueness is an open question

- At least one immortal path exists; **it is not known whether it is unique.**
- The OEIS sequence A367620 is *defined* as the **lexicographically earliest**
  infinite path: at each fork, take the smallest child *provided that subtree
  still contains an infinite continuation*. That tie-break makes one canonical
  sequence by definition, not by proof.
- Other infinite branches (e.g. always taking the larger child in some pattern)
  may or may not exist; the number of infinite branches of the 20-tree is
  unknown.

## 6. How the choice sequence (A399179) is derived

A399179 records, at each fork, whether the lexicographically earliest infinite
path takes the smaller (0) or larger (1) child. These bits cannot be computed
greedily: the firstborn path dies in base 10.

**Computational engine:** comma-numbers are periodic while the leading digit
doesn't change, so the sequence advances through each decade-block in
arithmetic-progression jumps (e.g. 2,171 steps in one step). Combined with the
fact that landmines only occur near powers of 10, this lets the computation
follow paths to ~10^84 terms.

**How a bit is certified — the pigeonhole argument.** A fork on the infinite
path has an infinite subtree, so at least one of its two children has an
infinite subtree (if both were finite, the fork's subtree would be finite,
contradicting the path's infinitude). Therefore:

- Prove the **smaller** child's subtree finite (exhaustively walk every path
  in it until each dies)  => larger must be infinite => **bit = 1**.
- Prove the **larger** child's subtree finite  => smaller infinite =>
  **bit = 0**.

The surviving branch is infinite by exactly the same reasoning that makes 20
infinite (49 finite trees + pigeonhole).

**Why it doesn't finish the sequence:**
1. A finiteness certificate is a huge exhaustive enumeration (every path in
   the losing subtree, including its own forks). Each published bit is one
   completed proof, which is why only ~30-60 bits are known.
2. If **both** children are infinite, neither branch admits a finiteness
   certificate and the bit is undeterminable by this method — the choice
   exists (take the smaller) but cannot be certified. This is the same
   unresolved uniqueness question.

**Provisionality:** the known bits are empirically the lexicographically
smallest path that has outlived all competitors so far. The OEIS records "one
of four candidates (all other possible starts having terminated)", "three
candidates left, but only one survived" at the 30th fork, and Sloane's comment
"We have not identified that binary sequence yet!". Existence of at least one
immortal path is proven; the correctness of any particular published bit
beyond its certificate's range is not.

## 7. Related results

- Base 3: all comma sequences are finite (proven), but the base-3 child graph
  has a **unique** infinite path, with choice sequence `010101...` (proven,
  Theorem 10.1 of the paper).
- The finiteness of all comma sequences in bases 3..633 is proven
  computationally by a finite-graph reduction (Dougherty-Bliss & Ter-Saakov,
  2024); base 10 is included via that method.

## 8. Key OEIS entries

- A121805 — the original comma sequence (start 1).
- A367341 — landmines (no successor).
- A367346 — numbers with two comma-children (the forks).
- A367620 — lexicographically earliest infinite path (starts 20, 22, 46, ...).
- A399179 — the binary decision sequence leading to immortality.
- A367338 — comma-successor of n.
- A121805's cross-refs give the fast-computation programs.

## 9. References

- Numberphile video, Sep 4 2026: "The Immortal Kangaroo Sequence".
- E. Angelini, M. Branicky, G. Resta, N. J. A. Sloane, D. W. Wilson,
  "The Comma Sequence: A Simple Sequence With Bizarre Properties",
  arXiv:2401.14346, Fibonacci Quarterly 62:3 (2024), 215-232.
- R. Dougherty-Bliss, N. Ter-Saakov, "The Comma Sequence is Finite in Other
  Bases", arXiv:2408.03434.
- N. J. A. Sloane, "Eric Angelini's Comma Sequence", Rutgers Experimental
  Math Seminar slides (Jan 2024): neilsloane.com/doc/EMJan2024.pdf