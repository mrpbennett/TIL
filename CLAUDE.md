# TIL Repository

A collection of short markdown documents about things learned across languages, frameworks, and technologies.

## README Structure

- **Lines 1–17**: Header and intro
- **Lines 19–73**: Table of Contents (navigation links only — never modify with dates)
- **Line 75 onward**: Content sections with TIL entries

## Add Dates to README Entries

After adding new entries to `README.md`, run this task to annotate each entry with the date it was added to git.

### What it does

Appends `(YYYY-MM-DD)` to every TIL list item in the content section, using `git blame --date=short` as the date source. Entries that already have a date are left alone.

### Rules

- Only list items matching `\s*- \[` get dates (both top-level and indented sub-items)
- The TOC section (lines 19–73) is never touched
- Headings, blank lines, and `---` separators are never touched
- Date format is always `(YYYY-MM-DD)` appended with a single space

### How to run

Ask Claude: "Add dates to README entries" — this will:

1. Run `git blame --date=short README.md` to get per-line dates
2. For each list entry after line 74 that doesn't already end with a date, append `(YYYY-MM-DD)`
3. Write the updated README.md
4. Show a diff for review

### Approach

Write a temporary Python script that:

1. Parses `git blame --date=short README.md` output, extracting dates per line number
2. Reads `README.md` line by line
3. For lines past the TOC (line 75+) matching `\s*- \[` that don't already end with `(YYYY-MM-DD)`, appends the blame date
4. Writes the result back and deletes itself
