# Instructions for Converting Handwritten Lecture Notes to LaTeX

## Overview

This project converts handwritten Math 132 (Differential Topology) lecture notes (scanned PDFs in `reference/lecture-notes/`) into formatted LaTeX with colored theorem boxes. Each lecture gets its own `.tex` file in `lectures/`, and `main.tex` combines them all.

## Project Structure

```
main.tex          — Master document (report class). Each lecture = one \chapter.
preamble.tex      — All packages, math macros, tcolorbox environments, colors.
titlepage.tex     — Course info title page.
lectures/         — One file per lecture (lecture01.tex, lecture02.tex, ...).
reference/        — Source scanned PDFs and textbook. Never edit these.
```

## Step-by-Step Conversion Process

### 1. Read the PDF

Read the new PDF from `reference/lecture-notes/`. Each lecture starts with "Math 132: Differential Topology" and a section title. Note the page numbers in the top-left corner (1/, 2/, 3/, etc.).

### 2. Create the Lecture File

Create `lectures/lectureNN.tex` (two-digit number). The file should **not** contain `\chapter` — that goes in `main.tex`.

### 3. Add the Chapter to main.tex

Add to `main.tex`:
```latex
\chapter{Title of Lecture}
\input{lectures/lectureNN}
```

### 4. Classify Content into Environments

Map handwritten labels to LaTeX environments:

| Handwritten Label | LaTeX Environment | Color |
|---|---|---|
| "Def" (underlined) | `\begin{definition}{Title}{label}` | Blue |
| "Prop" (underlined) | `\begin{proposition}{Title}{label}` | Amber |
| "Thm" or "Theorem" (underlined) | `\begin{theorem}{Title}{label}` | Orange |
| "Lemma" (underlined) | `\begin{lemma}{Title}{label}` | Teal |
| "Cor:" or "Corollary" | `\begin{corollary}{Title}{label}` | Purple |
| "Rmk" or "Remark" | `\begin{remark}{Title}{label}` | Gray |
| "Claim" | `\begin{claim}{Title}{label}` | Indigo |
| "Example" (underlined) | `\begin{example}{Title}{label}` | Green |
| "MetaDef" (red text) | `\begin{remark}{Meta-definition: Title}{label}` | Gray |
| "proof)" or "Pf:" | `\begin{proof} ... \end{proof}` | Gray left border |
| Section headers with § symbol | `\section{}` or `\subsection{}` | — |
| Red-underlined terms | `\emph{term}` | — |
| Boxed equations in notes | `\boxed{}` in display math | — |

Each tcolorbox environment takes the form:
```latex
\begin{theorem}{Title Goes Here}{short-label}
    Content here.
\end{theorem}
```

The `{short-label}` is used for cross-references (e.g., `\ref{thm:short-label}`).

### 5. Math Notation Conventions

Use the macros defined in `preamble.tex`:

| Notation | Macro | Output |
|---|---|---|
| Real numbers | `\R` | $\mathbb{R}$ |
| Natural numbers | `\N` | $\mathbb{N}$ |
| Integers | `\Z` | $\mathbb{Z}$ |
| Rationals | `\Q` | $\mathbb{Q}$ |
| Absolute value | `\abs{x}` | $|x|$ |
| Norm | `\norm{x}` | $\|x\|$ |
| Identity map | `\id` | id |
| Image | `\im` | im |
| Codimension | `\codim` | codim |
| Rank | `\rank` | rank |

Additional conventions:
- Function notation: use `\colon` instead of `:` — e.g., `f \colon X \to Y`.
- Composition: `g \circ f`.
- Derivative at x: `df_x`.
- Tangent space: `T_x M`.
- Partial derivatives: `\frac{\partial f_i}{\partial x_j}`.
- Jacobian matrix: display as a matrix with partial derivative entries.
- "s.t." in the notes means "such that" — write it as `s.t.` or spell out.
- "iff" means "if and only if" — use `\iff` in math mode.
- Set-builder notation: `\{ x \in \R^k \mid \text{condition} \}`.
- Transversality: use `\pitchfork` from amssymb.

### 6. Structural Conventions

- **Sections**: Use `\section{}` for major topic shifts (matching the § headers in the notes), `\subsection{}` for sub-topics.
- **Narrative text**: Motivational explanations and intuition should be regular paragraphs.
- **"Recall:" blocks**: Brief recap paragraphs at the start of lectures. Write as italic text.
- **Multi-step proofs**: Use `\textbf{Step 1.}`, `\textbf{Step 2.}`, etc.
- **Proof sketches**: `\begin{proof}[Proof sketch]`
- **References to textbook**: Keep verbatim (e.g., "See Chapter 1, §4 of Guillemin--Pollack").
- **"HW" or homework notes**: Write as "(see Homework)" or similar parenthetical.
- **Marginal annotations**: The notes sometimes have small annotations in the margin (e.g., "we need an open domain to talk about partial derivatives"). Include these as parenthetical remarks or brief inline notes.

### 7. Diagram Conventions

The notes contain several types of diagrams:

**Commutative diagrams** (most common): Use `tikz-cd`.
```latex
\[
\begin{tikzcd}
U \arrow[r, "f"] \arrow[dr, "g \circ f"'] & V \arrow[d, "g"] \\
& W
\end{tikzcd}
\]
```

**Simple manifold sketches**: Use TikZ with `\draw` commands.

**Complex topological illustrations** (tori, Mobius bands, saddle surfaces): Add a TODO comment:
```latex
% TODO: TikZ diagram of [description]
```

### 8. Cross-References

- Label format: `{short-descriptive-name}` — e.g., `{smooth-map}`, `{inv-fn-thm}`, `{tangent-space}`.
- When the notes say "proved last time" or "from last lecture", add a `\cref` if the referenced result has a label.
- The tcolorbox numbering is automatic (Definition 1.1, Theorem 1.2, etc.) — don't manually number.

### 9. Faithfulness to Source

**This is critical.** The LaTeX should be a faithful transcription of the handwritten notes:

- Include all content: definitions, theorems, propositions, proofs, examples, remarks, motivational text.
- Preserve the logical flow and ordering of the notes.
- If the professor crossed something out, omit it (it's a correction).
- If there's a boxed equation or result in the notes, use `\boxed{}`.
- Include intuition and marginal notes.
- Do not add content that isn't in the notes.
- Do not reorganize the material — keep it in lecture order.

### 10. Building the PDF

```bash
# Using pdflatex (run twice for TOC)
pdflatex main.tex && pdflatex main.tex

# Or using latexmk
latexmk -pdf main.tex
```

### 11. Quality Checklist

After converting a new lecture, verify:

- [ ] All definitions, theorems, propositions, lemmas are in colored tcolorbox environments
- [ ] Proofs use `\begin{proof}` and end with QED symbol
- [ ] Math notation uses the macros from preamble.tex
- [ ] No `\chapter` in the lecture file (only in main.tex)
- [ ] Content is faithful to the handwritten notes
- [ ] The file compiles without errors when included via main.tex
- [ ] New `\chapter` + `\input` added to main.tex
- [ ] Commutative diagrams rendered with tikz-cd
- [ ] Proposition environments used for "Prop" labels
- [ ] Red-underlined terms marked with `\emph{}`
