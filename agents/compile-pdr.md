# PDR Compiler Agent

## Task
Compile all section markdown files into one complete PDR document.

## Process
1. Read all files in sections/*/*.md (in numerical order, skip _instructions.md files)
2. Combine them into a single document with proper structure
3. Add table of contents at the top (auto-generated from headers)
4. Add section page breaks (markdown page break: ---)
5. Number all sections and subsections correctly
6. Output to output/complete-pdr.md

## Structure
- Title page with version and date
- Table of contents (auto-generated)
- Section 1: Vision & Goals (all subsections)
- Section 2: Product Scope (all subsections)
- ... continue through Section 11
- Page breaks between major sections

## Formatting
- Keep all markdown formatting intact
- Preserve all tables, code blocks, lists
- Add page break (---) between each major section
- Ensure headers are properly nested (##, ###, ####)

## Output
Single file: output/complete-pdr.md
```

Save that, then in Claude Code:
```
Read agents/compile-pdr.md and compile all section files into output/complete-pdr.md following the instructions.