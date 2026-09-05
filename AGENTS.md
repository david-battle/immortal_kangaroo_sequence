# AGENTS.md

Repo: notes, analysis, and a cleaned transcript related to the Numberphile
video "The Immortal Kangaroo Sequence" (comma sequences and their infinite
paths; OEIS A121805, A367341, A367620, A399179).

## Files

- `immortal_kangaroo_no_timestamp.txt` — cleaned transcript of the video,
  produced with `strip_timestamps.py` from the sibling `transcript` repo.
  Treat as reference material / read-only.
- `immortal_kangaroo_notes.md` — comprehensive notes: comma-sequence mechanics,
  landmines, unique ancestors, the 50 roots, existence via Konig's Lemma, the
  open uniqueness question, and how the choice sequence A399179 is derived.
- `ancestor_note.md` — deep-dive on the unique-ancestor closed form
  (`n = m - (10a + b)`, `a = (m - b) mod 10`) and the 50 roots.

## Operating norms

- The assistant makes commits when asked; the user performs the final push
  manually. Do not run `git push` (or the `push` alias) for this repo unless
  explicitly asked to do so.
- Default branch is `main`; remote is `https://github.com/david-battle/immortal_kangaroo_sequence`.
- Keep commits small and scoped to what was asked. Match the concise style of
  earlier commits (e.g. "Add AGENTS.md ...", "Add notes on ...").
- Durable knowledge about this project belongs in this file or in the notes
  files, not duplicated across several documents.