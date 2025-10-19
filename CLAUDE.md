# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with
code in this repository.

## Project Overview

This is a LaTeX project for creating Dutch spelling and grammar exercises
("vind de fouten" - find the errors) for an 11-year-old student in groep 8
(final year of Dutch elementary school). The goal is to create engaging
exercises where students identify errors in Dutch texts through multiple-choice
questions.

## Exercise Format

Each exercise consists of:
1. A Dutch text with intentional spelling/grammar errors, displayed with line
   numbers
2. Approximately 5 multiple-choice questions that:
   - Reference specific parts of the text by line number
   - Offer 4 options: the original text ("as-is") + 3 alternative corrections
   - May have any of the 4 options as correct (including "as-is" for parts
     that are already correct)

## Build Commands

```bash
# Generate the list of exercise files
./genfilelist

# Compile the main document to PDF
pdflatex vind_de_fouten.tex

# If packages are missing, install them:
# sudo apt install texlive-lang-european texlive-latex-extra
```

## Project Structure

```
/
├── vind_de_fouten.tex   # Main LaTeX document
├── exercises/           # Individual exercise files (exercise0001.tex, etc.)
├── filelist.tex        # Auto-generated list of exercises (by genfilelist)
├── genfilelist         # Script to generate filelist.tex
├── TOPICS.md           # List of exercise themes
├── COMMON_ERRORS.md    # Reference guide for Dutch spelling/grammar errors
└── README.md           # Detailed project requirements
```

## Document Architecture

The project uses a two-pass system to separate exercises and answer keys:

1. **vind_de_fouten.tex** uses conditional compilation with `\ifshowopgave`
   and `\ifshowoplossing`
2. Exercise files contain both `\begin{opgave}...\end{opgave}` and
   `\begin{oplossing}...\end{oplossing}`
3. The main document includes `filelist.tex` twice:
   - First pass: `\showopgavetrue` and `\showoplossingfalse` (shows only
     exercises)
   - Second pass: `\showopgavefalse` and `\showoplossingtrue` (shows only
     answer keys in appendix)

## Development Guidelines

### Exercise Creation
- Each exercise goes in `exercises/exerciseXXXX.tex` (4-digit numbering,
  starting from 0101)
- Each file must contain both:
  - `\begin{opgave}...\end{opgave}` - The text with errors and
    multiple-choice questions
  - `\begin{oplossing}...\end{oplossing}` - Answer key with explanations
- **CRITICAL WORKFLOW**: After creating EACH SINGLE exercise, immediately run:
  1. `./genfilelist` to update filelist.tex
  2. `pdflatex vind_de_fouten.tex` to test the build
  3. Fix any LaTeX errors before proceeding to the next exercise

### LaTeX Structure Within Exercises

Each exercise file should follow this structure:

```latex
\begin{opgave}

\begin{tekstmetfouten}
Line 1 of text with explicit line break.\\
Line 2 of text with explicit line break.\\
...more lines...\\
Final line of text (no \\ needed on last line).
\end{tekstmetfouten}

\begin{vragen}
\vraag{line-number}{text fragment}{focus phrase}%
{option A with "(zoals in de tekst)" if original}%
{option B}%
{option C}%
{option D}

% More questions...
\end{vragen}

\end{opgave}

\begin{oplossing}
\begin{enumerate}
\item A - Explanation for question 1
\item B - Explanation for question 2
% More answers...
\end{enumerate}
\end{oplossing}
```

**Key environments:**
- `tekstmetfouten` - Displays text with line numbers (from lineno package)
- `vragen` - Container for questions (adds "Vragen:" header)
- `\vraag` command - Creates individual questions in unbreakable minipage boxes
- `oplossing` - Answer key with enumerated list

### Text and Error Guidelines
- **Text display**: Use a numbered line environment to show the source text
  with line numbers
- **CRITICAL: Explicit line breaks**: Each line in the `tekstmetfouten`
  environment MUST end with `\\` to create explicit line breaks. This ensures
  the line numbers in the PDF match the line numbers referenced in questions.
  Without explicit breaks, LaTeX will reflow text and line numbers won't
  correspond
- **CRITICAL: One-to-one error-question mapping**: Every spelling or grammar
  error in the text MUST have a corresponding question. Conversely, do NOT
  include errors in the text that are not covered by a question. The text may
  contain correct usage (for verification questions), but incorrect usage must
  always be questioned
- **Error selection**: Refer to COMMON_ERRORS.md for documented Dutch spelling
  and grammar error patterns to use in exercises
- **Error types**: Include various Dutch spelling and grammar errors:
  - dt-errors (werkwoordspelling)
  - d/t confusion at word endings
  - ei/ij confusion
  - Compound words (open vs closed)
  - Comma placement
  - Capital letters (proper nouns, sentence starts)
  - their/there equivalents (hun/hen, der/daar, etc.)
  - Agreement errors (subject-verb, noun-adjective)
- **Error distribution**: Not every line needs an error; mix correct and
  incorrect parts
- **Question format**: Each question should:
  - Use `\vraag{line number}{text fragment}{focus phrase}{optionA}{optionB}{optionC}{optionD}`
  - Line number can be single (1) or range (1-2)
  - Text fragment should be at least one full sentence, using ellipsis (...)
    to show beginning and ending without copying everything
  - Example text fragment: "Ik vondt een verlaten mijnschacht ... de grond."
  - Focus phrase identifies the specific part to evaluate (e.g., "vondt")
  - This appears as "Wat moet je doen met '[focus phrase]'?"
  - Options should show ONLY the variable part, not repeated context
  - Option A should include "(zoals in de tekst)" when it shows the original
    text
  - Offer 4 clearly distinct options
  - Have exactly one correct answer

### Answer Key Format
- Use `\begin{enumerate}...\end{enumerate}` for answer lists
- Each item lists the correct answer letter and brief explanation
- For correct "as-is" answers, explain what rule makes it correct
- Keep explanations concise but educational
- Example:
  ```latex
  \begin{enumerate}
  \item B - Correct spelling is "bijvoorbeeld" (compound word)
  \item A - Original is correct (no dt-rule applies here)
  \item C - vindt is correct (3e persoon enkelvoud: hij vindt)
  \end{enumerate}
  ```

### Content and Themes
- Use UTF-8 encoding
- **Avoid Unicode characters**: Use text equivalents instead of symbols
- **Escape special characters**: Use `\&` instead of `&` in text
- **Line length**: Keep source files to maximum 100 characters per line
- Theme exercises around topics interesting to the target audience:
  - Gaming (Minecraft, Zelda, Fortnite)
  - Tennis, scouts
  - Harry Potter
  - Cooking, saxophone
  - Board games (Scrabble, Catan, Monopoly, Everdell)
  - Ice cream, swimming
  - Sinterklaas
  - Travel within Netherlands and to France
  - Avoid soccer themes

### Important Requirements
- Every question MUST have exactly one correct answer
- Include both error-finding AND verification of correct usage
- Errors should be realistic mistakes that Dutch students actually make
- Questions should target groep 8 level (age 11-12)
- Text length should be approximately 22-26 lines with ~5 errors (lower error
  density allows testing of verification skills on correct text)
- Difficulty should be appropriate for VWO-level preparation

### Document Requirements
- A4 paper, portrait orientation
- 12pt font size
- 1-inch margins (already configured)
- Exercises numbered automatically
- No page numbering needed
- **Font configuration**: Uses Latin Modern fonts with T1 encoding for proper
  Dutch character rendering
- **Paragraph formatting**: No indentation, half-line spacing between
  paragraphs

### Layout and Spacing
- **Page breaks**: Each exercise starts on a new page (automatic via
  `\clearpage` in opgave environment)
- **Question protection**: Each question is wrapped in a minipage to prevent
  page breaks within a question
- **Spacing**: Questions use `\bigskip` before and after for clear visual
  separation

## Release Process

The project uses GitHub Actions to automatically build and release PDFs:

1. Create and push a version tag:
   ```bash
   git tag v0.1.0
   git push origin v0.1.0
   ```

2. GitHub Actions will:
   - Install LaTeX dependencies
   - Run `./genfilelist`
   - Build PDF with `pdflatex vind_de_fouten.tex` (twice for cross-references)
   - Create a GitHub release with the PDF attached

The workflow is configured in `.github/workflows/release.yml`.

## Target Audience
- 11-year-old Dutch boy preparing for VWO-level secondary education
- Needs practice with Dutch spelling and grammar rules
- Motivated by engaging, fun themes (avoid soccer themes)

## Tips for Future Sessions
- Always check COMMON_ERRORS.md for error type inspiration
- Mix different error types across exercises for variety
- Test build after each exercise to catch LaTeX errors early
- Ensure every error in the text has a corresponding question
- Keep text themes diverse and engaging
