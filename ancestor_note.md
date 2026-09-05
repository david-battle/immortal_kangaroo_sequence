# The Comma-Sequence Ancestor: Why It Is Unique (and Computable)

The Numberphile video hand-waves this: "every path comes back to a number
between 1 and 99." It's true, and it falls out of a one-line closed form with a
clean mod-10 argument. There is exactly one parent per number.

## Setup

In the comma sequence, consecutive terms `n` (parent) and `m` (child) satisfy

```
m = n + s
```

where `s` is the comma separator: a two-digit number whose tens digit is the
last digit of `n` and whose ones digit is the first digit of `m`. Call those
digits `a` and `b`. So `s = 10a + b`, and

```
m = n + 10a + b        (a = last digit of n,  b = first digit of m)
```

## Computing the parent of m

```
b = first digit of m            (fixed by m alone)
a = (m - b) mod 10              (forced, see below)
n = m - (10a + b)
```

## Uniqueness (why `a` is forced)

`b` is the first digit of `m`, so it is fixed once `m` is known.

We need `a` to equal the last digit of `n = m - (10a + b)`. But subtracting a
multiple of 10 does not change a number's last digit, so

```
last digit of n = last digit of (m - b) = (m - b) mod 10
```

Hence `a = (m - b) mod 10` is the *only* possible value. There is no freedom.
Then `n` is uniquely determined. By construction `n`'s last digit is `a` and the
separator `(a)(b)` equals `m - n`, so it is genuinely a comma-step.

## Existence

The only failure mode is `n < 1` (no positive parent). Otherwise `n >= 1` is a
valid parent, and there is exactly one.

## The subtlety that hides the simplicity

A *parent can have two children* (the forks, e.g. 14 -> 59 and 60), but *a child
has exactly one parent*. Backward tracing is deterministic even though forward
paths can branch. Do not mistake a parent's second child for a root.

(A naive check using the "smallest child" rule wrongly treats the larger child
of a fork as root — a bug, not a counterexample. E.g. 60 is a child of 14, whose
firstborn is 59.)

## The roots

Running the formula over 1..99 yields exactly 50 numbers with no parent
(where the computed `n < 1`):

```
1..10, 13..21, 25..32, 37..43, 49..54, 62..65, 74..76, 86..87, 98
```

Every positive integer, traced up, lands on one of these, giving 50 trees —
matching the video's "50 components." Of these, 20 is the only immortal start.
