# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a LaTeX project for creating Dutch math story problems ("verhaalsommen") for an 11-year-old student in groep 8 (final year of Dutch elementary school). The goal is to create engaging exercises that are challenging enough to require written calculation rather than mental math.

## Build Commands

```bash
# Verify calculations for exercises
./calc "expression"

# Generate the list of exercise files
./genfilelist

# Compile the main document to PDF
pdflatex verhaalsommen.tex

# If packages are missing, install them:
# sudo apt install texlive-lang-european texlive-latex-extra
```

## Project Structure

```
/
├── verhaalsommen.tex    # Main LaTeX document
├── exercises/           # Individual exercise files (exercise0001.tex, etc.)
├── filelist.tex        # Auto-generated list of exercises (by genfilelist)
├── genfilelist         # Script to generate filelist.tex
├── TOPICS.md           # List of exercise themes
└── README.md           # Detailed project requirements
```

## Document Architecture

The project uses a clever two-pass system to separate exercises and solutions:

1. **verhaalsommen.tex** uses conditional compilation with `\ifshowopgave` and `\ifshowoplossing`
2. Exercise files contain both `\begin{opgave}...\end{opgave}` and `\begin{oplossing}...\end{oplossing}`
3. The main document includes `filelist.tex` twice:
   - First pass: `\showopgavetrue` and `\showoplossingfalse` (shows only exercises)
   - Second pass: `\showopgavefalse` and `\showoplossingtrue` (shows only solutions in appendix)

## Development Guidelines

### Exercise Creation
- Each exercise goes in `exercises/exerciseXXXX.tex` (4-digit numbering)
- Each file must contain both:
  - `\begin{opgave}...\end{opgave}` - The question
  - `\begin{oplossing}...\end{oplossing}` - Step-by-step solution showing work, ending with "Antwoord: [final answer with units]"
- **CRITICAL WORKFLOW**: After creating EACH SINGLE exercise, immediately run:
  1. `./genfilelist` to update filelist.tex
  2. `pdflatex verhaalsommen.tex` to test the build
  3. Fix any LaTeX errors before proceeding to the next exercise
- **Solution format**: Show step-by-step calculations with intermediate results, then conclude with "Antwoord: [final answer with units]"
- Solutions must include natural units when applicable (€, rupees, punten, gram, meter, etc.)
- **ALWAYS verify calculations using ./calc**: `./calc "3 + 2 * 6"` yields 15
- Use UTF-8 encoding, avoid special multiplication symbols (use 'x' instead of '×')
- **Avoid Unicode characters**: Never use Unicode symbols like ✓, ≈, ×, etc. - use text equivalents instead
- **Exercise formatting variety**: Mix different formatting styles for visual variety:
  - Some exercises can use bullet lists with `\begin{itemize}\item...\end{itemize}` 
  - Others can use paragraph format without bullets
  - Both styles are acceptable and should be mixed throughout
- **Escape special characters**: Use `\&` instead of `&` in text
- **Line length**: Keep source files to maximum 100 characters per line for readability
- Theme exercises around: gaming (Minecraft, Zelda, Fortnite), tennis, scouts, Harry Potter, cooking, saxophone, explosions, mud, board games (Scrabble, Catan, Monopoly, Everdell), Daan's Droomijs (ice cream shop), Sinterklaas, Knopenbad (swimming pool), traveling by car within Netherlands and to France

### Important Requirements
- Every question MUST have a clear, numeric answer
- Answers should never be "none", "you can't do that", or similar - always a specific number
- If a problem seems impossible, adjust the starting amounts to make it solvable with a numeric answer
- Focus on multi-step problems with simple individual operations:
  - Example: Restaurant bill with multiple items (2 × €3.50 + 4 × €2.25 + 3 × €5.50)
  - Each step is simple, but tracking multiple intermediate results makes paper helpful
  - Avoid single complex operations like 3-digit multiplication or long division
  - The goal is problems that are technically doable mentally but much more reliable on paper
- **Variety in calculation techniques**: Include diverse problem types across exercises:
  - Division with remainders, percentages, fractions, ratios, averages
  - Time calculations, unit conversions, area/volume problems
  - Pattern recognition, logical reasoning with numbers
  - Problems requiring multiple different operations in sequence

### Document Requirements
- A4 paper, portrait orientation
- 12pt font size
- 1-inch margins (already configured)
- Exercises numbered automatically
- No page numbering needed
- **Font configuration**: Uses Latin Modern fonts with T1 encoding for better Euro symbol rendering
- **Paragraph formatting**: No indentation, half-line spacing between paragraphs

## Target Audience
- 11-year-old Dutch boy preparing for VWO-level secondary education
- Strong at math but needs practice with written calculation
- Motivated by engaging, fun themes (avoid soccer themes)