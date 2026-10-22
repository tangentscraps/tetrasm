# tangentscraps/tetrasm

> A learntris implementation in NASM for FreeBSD x64 that never got past
> "echo input back": a 30-line assembly scaffold with syscall macros, built
> in one day.
> Repo: https://github.com/tangentscraps/tetrasm — 6 files, 7 commits, all
> 2013-07-19. **Not archived.**

## What it is

**Tetrasm** was supposed to be [learntris](https://learntris.org) in assembly —
the tagline says so right in the repo description ("learntris implementation in
nasm for freebsd 64") and the README. What actually exists is a minimal I/O
scaffold: `tetrasm.asm` is 30 lines that read up to 64 bytes from stdin into a
buffer and write them back to stdout, then exit. That's it — no matrix, no
tetriminos, no scoring. It implements the *input echo* and nothing else.

The commits read like a language warmup exercise: "initial version: simply
exits" → "created a Makefile" → "added freebsdx64.asm for platform-specific
stuff and moved the exit() syscall into a macro" → "created an iobuffer and
macros for sys.read/write/exit". Seven commits, all on 2013-07-19, by
tangentstorm alone. One day of "let me get the I/O plumbing right," then
nothing ever again.

(Flagged as unverified: the repo metadata shows an updated stamp of
2025-08-07, but every commit in the history is dated 2013-07-19, so the 2025
stamp likely reflects a metadata/setting change rather than new code.)

## Tech stack

NASM (Intel-syntax, `-felf64`), linked with plain `ld`, no libc, no startup
code. System calls use **FreeBSD x86-64** numbers (1 = exit, 3 = read,
4 = write) — the macros in `freebsdx64.asm` would not work on Linux, where the
syscall numbers differ. Platform separation was already on his mind: the commit
message says the platform-specific bits were split into `freebsdx64.asm`
deliberately. MIT licensed.

## Inventory

| Path | Contents |
|------|----------|
| `tetrasm.asm` | 30 lines: read 64 bytes from stdin, write them to stdout, exit |
| `freebsdx64.asm` | `%define` register aliases + `sys.exit` / `sys.read` / `sys.write` macros |
| `Makefile` | `nasm -felf64` + `ld` rules; `run` / `clean` targets |
| `README.md` | Two lines: "tetrasm" / "learntris implementation in nasm for freebsd 64" |
| `LICENSE` | MIT, copyright 2013 tangentstorm |
| `.gitignore` | Build artifacts (`*.o`, the `tetrasm` binary) |

## Surprising / notable

- **One day, zero learntris:** the ambitious title vs. the echo-loop reality is
  the whole joke of the repo. It belongs to the 2013-era family of Tetris
  variants (tetraminex, tetrasm) that fed the later learntris spec work.
- **FreeBSD targeting** in 2013 is a distinctive choice — most assembly hobby
  projects target Linux; the FreeBSD syscall convention (via `syscall` with
  these register/number assignments) is baked into the macro file.
- **Commit messages tell the story** of someone scaffolding before thinking:
  iobuffer and syscall macros were the "interesting" part, the actual
  learntris never started.
- The repo is **not archived** (unlike most tangentscraps repos triaged so
  far) — a deliberate someday lives elsewhere: GTD someday item #17 is
  **"Re-do tetrasm (learntris in assembly)"**.
- No linked streams or videos found in the repo. Verdict at triage: **document**
  (done — this file).

## Verdict

Keep (unarchived), document. The someday re-do (#17) is the natural next step
whenever he wants a small assembly project again — the I/O plumbing is done,
the learntris spec is at https://learntris.org, so a re-do starts from actual
game logic this time.
