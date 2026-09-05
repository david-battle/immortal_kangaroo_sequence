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

### AC: a clean "for" example, and the symmetric "against" example

The kangaroo is a crisp illustration of why one *might want* the Axiom of
Choice: it grants non-constructive existence of the immortal path. The
mirror-image argument *against* full AC is the **Banach–Tarski paradox** and
its engine, the **Vitali set**, where choice forces some subset of the circle
to have no well-defined length:

- Partition the unit circle by `x ~ y` iff one is a rational rotation of the
  other. Pick one representative per class (that's the AC move) to get a
  Vitali set `V`.
- Rational translates of `V` tile the circle; by additivity its "length" must
  equal a countable sum of equal terms, which is impossible unless it is 0 —
  yet a sum of 0s can't be the circle's circumference. So `V` is
  non-measurable. Banach–Tarski then reassembles a ball into two copies from
  finitely many such pieces.

Both examples are *consistent* facts about choice, not contradictions: ZF alone
doesn't settle them, and you may consistently add AC *or* "all sets are
Lebesgue measurable" (Solovay's model). The contrast:

| | Immortal kangaroo | Vitali set / Banach–Tarski |
|---|---|---|
| What AC does | Proves existence of an infinite path (nice) | Produces pathological, non-measurable sets (weird) |
| Why | Non-constructive branch in a finitely-branching tree | Non-constructive selection, breaks measurability |
| Strength needed | Only the **axiom of dependent choice (DC)** | Stronger fragment of choice |

Key nuance: the kangaroo proof needs only **DC**, a weak fragment of AC, so
"pick a branch" can be satisfied without the full choice that Banach–Tarski
needs. Accepting DC while rejecting full AC keeps both the useful existence
theorem and Lebesgue measurability — a common middle-ground position.

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

Concrete anchors: the first choice point (fork) occurs at
a(412987860) = 19999999918, i.e. only after ~4.1 x 10^8 terms does the first
binary decision arise. The sequence A367620 was discovered by David W. Wilson
in 2007.

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

## 9. Digression: AC, P vs NP, and "new axioms" (from discussion)

These are notes from a conversation that grew out of the AC discussion above;
they stray from the comma sequence but record the reasoning, including the
corrections the discussion forced.

### 9.1 AC is irrelevant to P vs NP

- P vs NP is a statement about **finite objects**: "∃ TM M, ∃ polynomial p,
  ∀ x, M decides SAT within p(|x|)". It is a **Σ⁰₂ arithmetic sentence**.
- AC only governs infinite/high-cardinality set-theoretic operations; it never
  touches a finite computation. Arithmetic statements are **absolute**: their
  truth is fixed by ℕ alone, independent of the set-theoretic universe. So
  AC (or any fragment, even none) cannot change the truth of P=NP.
- **Shoenfield absoluteness**: every Σ¹₂ sentence (hence every arithmetic
  sentence) provable in ZFC is already provable in ZF. So no proof of P vs NP
  can essentially depend on choice. The door "prove it with choice but not
  without" is closed.
- Caveat: Shoenfield covers ZFC-vs-ZF only. *Beyond* ZFC, strong axioms can
  decide some arithmetic sentences (e.g. an inaccessible proves Con(ZFC), a
  Π⁰₁ sentence). Whether any natural large-cardinal axiom decides P vs NP is
  **genuinely open**.
- Contrast with our earlier examples: as the objects become more finite, choice
  goes from essential (kangaroo/DC) → optional (Banach–Tarski) → irrelevant
  (P vs NP).

### 9.2 Forcing is powerless; independence would be arithmetic, not CH-like

- P vs NP is **forcing-absolute** (arithmetic). Unlike CH, no forcing extension
  changes its truth. So if it is independent, the independence is *not*
  "CH-like" (a genuine choice about the set universe); it is **arithmetic
  incompleteness** — a failure of ZFC about the single structure ℕ, which has a
  determinate fact of the matter.
- This is the crux that separates P vs NP from AC/CH: AC and CH are
  **constitutional** choices about the shape of the set universe (both sides
  coherent and productive); P vs NP is a **matter of fact about ℕ**.

### 9.3 "Make P ≠ NP a new axiom" — the case against and the correction

Initial (too-strong) claim: "We resolve matters of fact by proof, not by
amendment." **This is false, and Gödel's G refutes it**: G is a true, Π⁰₁,
arithmetic matter of fact, unprovable in PA, resolvable only by moving to a
stronger system (PA + Con(PA), or ZFC). So amendment genuinely is how some
arithmetic facts get resolved. The real distinction is **principled amendment
vs ad hoc stipulation**, not "provable vs amendable."

**Why amending for G is principled (and P≠NP initially seemed ad hoc):**
- *Self-justifying*: Con(PA) vouches for the very system it extends; accepting
  it is compelled by believing PA coherent, not a bet on a contested fact.
- *Cascading*: it climbs a tower (PA ⊂ PA+Con(PA) ⊂ ... ⊂ ZFC ⊂ ...), a general
  reflection schema, each rung productive and revealing a new target.
- *Amendment supplies axioms; the fact then becomes provable in the new system.*
  It's not stipulating the fact; it's finding the next principle under which the
  fact is a theorem.

**Why P≠NP-as-axiom seemed ad hoc by the same test:**
- *Not self-justifying*: its truth is precisely what's in question.
- *Terminal, not cascading*: no new "P'≠NP" appears, no tower, no general schema.
- *No principled host*: no known natural general principle has P≠NP as a
  consequence.

**The correction (the user's point):** if P vs NP were **provably independent
of ZFC**, the situation changes categorically:
- It stops being a bet — an independence proof *certifies* ZFC is silent either
  way, removing the "you might be wrong" objection.
- It becomes *like the G ladder*: P≠NP is best read as **a theorem of a stronger
  system we haven't yet identified**, not a bare stipulation. The only reason it
  looks like a stipulation now is that we lack that stronger system in hand.
- Caveat: the quality of the independence proof matters. Robust, forcing-absolute
  independence supports the "next foundation" reading; a weak/pathological
  independence proof is less compelling.
- Bottom line: it is **not "is it a matter of fact" that separates these cases,
  but "do we have a certified silence of ZFC, or just a guess."** Certification
  flips the verdict.

### 9.4 The "prophetic" reading (Hofstadter, GEB) — and why it's a loose rhyme

The user's direction intuition: every barrier result has pushed toward
independence, making GEB prophetic.

**What supports it:**
- Relativization (Baker–Gill–Solovay), natural proofs (Razborov–Rudich), and
  algebraization (Aaronson–Wigderson) each *eliminated a family of proof
  techniques*. The systematic failure of every broad lower-bound method is real
  evidence the proof (if it exists) won't look like anything known — and it
  rhymes with GEB's thesis that a system's power and limits are intertwined.

**What undercuts it:**
- Barriers apply to *independence proofs too* — an independence result faces the
  same gauntlet, so barriers don't differentially favor independence over "just
  very hard."
- Historical pattern is new *techniques* (non-relativizing, non-natural,
  non-algebrizing), not new *axioms* — momentum is toward more cleverness, not
  toward legislating.
- **P vs NP has no self-reference.** GEB's core is self-reference (G says "I am
  not provable"); P vs NP has none — no fixed point, no diagonalization over its
  own provability. Its independence would be arithmetic incompleteness, a
  different phenomenon from Gödelian self-reference, and we have no known method
  to even produce that kind of independence (forcing is powerless).
- Verdict: the intuition is a live hypothesis, not a trend line. Barriers cut
  both ways; GEB is a loose rhyme, not a roadmap.

### 9.5 The user's 90's insight: a hint of self-referential structure

(Loose, not a proof — the user knows this. Recorded as an intuition.)

- **P ≠ NP ⇒ OWFs** (loosely; the direction P≠NP ⇒ OWF is not known
  unconditionally, but OWF ⇒ P≠NP *does* hold). Hardness of *deciding* is
  entangled with hardness of *inverting*.
- **OWFs ⇒ near-perfect hashing** (loosest link; note tension: a one-way hash
  hides the structure a "fake fast algorithm" would need to exploit).
- **Perfect hashing ⇒ fast NP-complete algorithms**: the concrete seed. A
  perfect hash isolating a unique satisfying assignment is the territory of
  **downward self-reducibility** and **Valiant–Vazirani isolation** — real,
  studied self-referential-feeling structures: "decide if there's a solution by
  isolating a unique one via hashing."

**The genuine kernel of self-reference:** P vs NP asks whether *deciding* can
outrun *verifying* — "can the machine that recognizes truth also find truth?" —
a structural echo of self-reference. The concrete, honest instance is
**downward self-reducibility** (SAT decides by reducing to smaller SAT), which
is real mathematics.

**But the direction of the reflex is productive, not toward independence:**
self-reducibility and isolation are *tools for algorithms*, not a doorway out of
ZFC. The self-structure found so far is algorithmic and productive. The
self-reference P vs NP exhibits is *computational-reflex*, not
*formal-incompleteness* — which is exactly why the intuition doesn't lead to
independence. It's a good intuition to hold; it's just not the Gödelian kind.

## 10. References

- Numberphile video, Sep 4 2026: "The Immortal Kangaroo Sequence".
- E. Angelini, M. Branicky, G. Resta, N. J. A. Sloane, D. W. Wilson,
  "The Comma Sequence: A Simple Sequence With Bizarre Properties",
  arXiv:2401.14346, Fibonacci Quarterly 62:3 (2024), 215-232.
- R. Dougherty-Bliss, N. Ter-Saakov, "The Comma Sequence is Finite in Other
  Bases", arXiv:2408.03434.
- N. J. A. Sloane, "Eric Angelini's Comma Sequence", Rutgers Experimental
  Math Seminar slides (Jan 2024): neilsloane.com/doc/EMJan2024.pdf