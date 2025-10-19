# vind_de_fouten / find the errors - Dutch spelling and grammar exercises

This is a LaTeX project to create a series of Dutch spelling and grammar
exercises ("vind de fouten" - find the errors) for a Dutch student in the
final year of elementary school ("groep 8"), preparing for VWO-level
secondary education.

## Exercise Format

Each exercise consists of:
1. A Dutch text (22-26 lines) containing intentional spelling/grammar errors,
   displayed with line numbers
2. Approximately 5 multiple-choice questions that:
   - Reference specific parts of the text by line number
   - Offer 4 options: the original text ("as-is") + 3 alternative corrections
   - May have any of the 4 options as correct (some parts of the text are
     deliberately correct to test verification skills)
3. An answer key with explanations

## Error Types

The exercises include common Dutch spelling and grammar errors:
- dt-errors (werkwoordspelling)
- d/t confusion at word endings
- ei/ij confusion
- Compound words (open vs closed)
- Comma placement
- Capital letters (proper nouns, sentence starts)
- hun/hen, der/daar confusion
- Agreement errors (subject-verb, noun-adjective)

## Technical Implementation

The exercises have been created using:
- **Claude Code** - for generating exercises with realistic errors and
  educational value
- **LaTeX structure** - allows individual exercise files while keeping
  exercises and answer keys separated in the final output
- **Tools**:
  - `./genfilelist` - updates the list of exercises to be included
  - Custom LaTeX environments for:
    - `tekstmetfouten` - displays text with line numbers
    - `vragen` - formats multiple-choice questions
    - `\vraag` command - structures individual questions with 4 options

## LaTeX Structure

The project uses a two-pass system:
1. First pass: displays all exercises (text + questions)
2. Second pass: displays answer keys in an appendix

Each exercise file (`exercises/exerciseXXXX.tex`) contains:
- `\begin{opgave}...\end{opgave}` - the exercise (text and questions)
- `\begin{oplossing}...\end{oplossing}` - the answer key with explanations

## Local Build Process

```bash
./genfilelist
pdflatex vind_de_fouten.tex
```

## Release Process via GitHub Workflow

```bash
git tag v0.2.0 && git push origin v0.2.0
```

## Themes

Exercises are themed around topics interesting to the target audience:
- Gaming (Minecraft, Zelda, Fortnite)
- Sports (tennis, scouts)
- Harry Potter
- Hobbies (cooking, saxophone)
- Board games (Scrabble, Catan, Monopoly, Everdell)
- Daily life (ice cream shops, swimming pools, Sinterklaas)
- Travel (within Netherlands and to France)
