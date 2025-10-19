# verhaalsommen / math story exercises

This is a LateX project to create a series of math story exercises ("verhaalsommen")
for a Dutch student in the final year of elementary school ("groep 8"), tailored
towards his personal interests.

The math story exercises have been created in bulk by using
- Claude Code
- a LateX structure that allows us to create the exercises in individual files, while
  still keeping exercise and solution separated in the final output file
- some tools that we instruct Claude Code to use:
  - `./calc` to verify calculations; this greatly improves reliability
  - `./genfilelist` to update the list of exercises to be included

# local build process

```
./genfilelist
pdflatex verhaalsommen.tex
```

# release process via GitHub workflow
```
git tag v0.1.1 && git push origin v0.1.1
```
