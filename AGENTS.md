# AGENTS.md

A LaTeX book by Supragya Raj: *Applied Learnings for Adverserial Distributed Systems*.
The entry point is `book.tex`, and the compiled PDF is committed at `build/book.pdf`.

## Git

- Commit as **Supragya Raj <supragyaraj@gmail.com>**. This repo's local git config
  already sets that. Don't override it with `--author` or environment variables.
- Only commit or push when asked. Commit messages follow the existing style:
  `add: <topic>` or `fix: <topic>`.

## Building

You need TeX Live (MacTeX on macOS, `brew install --cask mactex`). The book builds
with pdfLaTeX through `latexmk`. Run this from the repo root:

```sh
latexmk -pdf -outdir=build book.tex
```

- `makeindex` runs automatically through `imakeidx`.
- `build/` is tracked. Commit a rebuilt `build/book.pdf` with any content change.
  Leave the intermediate files (`.aux`, `.log`, etc.) as `latexmk` writes them.
- To check that a change compiles, read `build/book.log` for `! ` errors and
  `undefined` references, not just the exit code.

## Layout

```
book.tex                  preamble + \import of each part, in order
bookstyle.sty             colours, fonts, \code, \todo, quotebox/refbox/alertbox
listings-{rust,solidity}.sty   language definitions for lstlisting
cover.tex                 title page
<part>/<part>.tex         one file per part; \subimport's its chapters
<part>/<topic>/<topic>.tex     a \chapter; may \subimport sections
<part>/<topic>/assets/    images for that chapter
```

Parts are `zero-knowledge`, `version-control`, `time-synchrony-failures`,
`adverserial-distributed-systems`, `programming-languages`, `problem-solving` and
`important-algorithms`.
The directory name `adverserial` is misspelled, but leave it as is because paths
depend on it.

To add a chapter, create `<part>/<topic>/<topic>.tex` starting with `\chapter{...}`,
then add `\subimport{./<topic>/}{<topic>.tex}` to the part file.

## Writing conventions

- Use `\subimport` (not `\input`), so relative paths resolve from each file's directory.
  `\includegraphics` paths in this repo are relative to the importing part directory,
  so copy an existing figure in the same chapter when you add an image.
- Inline code goes in `\code{...}`, and placeholders in `\todo{...}`. Both are defined in `bookstyle.sty`.
- Use code blocks with `\begin{lstlisting}[language=Rust|Solidity|..., caption=..., label=lst:...]`.
- Use these callouts: `quotebox{Title}`, `refbox{Further Resources}` for links at the
  end of a chapter, and `alertbox{Title}`.
- Exercises use `\begin{Exercise}[title={...}] ... \end{Exercise}`, followed by
  `\begin{Answer} ... \end{Answer}`, with an `enumerate` inside each.
- Add `\index{Term}` next to key terms on first definition.
- Label sections `sec:...` and listings `lst:...`.
- Open topics live in the root `TODO.md`. Add to it rather than leaving stray notes in `.tex`.
