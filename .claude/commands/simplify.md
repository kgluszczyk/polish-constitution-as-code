# /simplify — Propose a plain-language version of an article or chapter

Given an article or chapter number, produce a simplified, modern plain-language
restatement of the constitutional provision — in both Polish and English.

## Input

`$ARGUMENTS` — an article number (e.g. `55`, `31`) or chapter number prefixed
with "chapter" or "rozdział" (e.g. `chapter 4`, `rozdział II`).
If no argument given, ask the user what to simplify.

## Steps

### 1. Identify scope

- If the input is an article number → simplify that single article.
- If the input is a chapter → simplify every article in that chapter that has
  an implementation in the codebase (skip articles without code).

### 2. Gather the original text

For each article in scope:

a) Read the **bilingual docstring** from the Python implementation in
   `src/konstytucja/chapter_XX_*.py`. This is the authoritative legal text.
b) If the article is not yet implemented, read the text from
   `akn/konstytucja_rp.xml` (`<article eId="art_N">`).
c) If neither exists, note it and skip.

### 3. Analyse the legal language

Before rewriting, identify:
- **Archaisms** — words/phrases that have modern equivalents
- **Legalese** — unnecessarily complex constructions
- **Cross-references** — e.g. "z zastrzeżeniem ust. 2" (subject to para. 2)
- **Implicit assumptions** — things a non-lawyer wouldn't know
- **Nested conditions** — deeply nested if/then logic

### 4. Write the simplified version

Produce a plain-language restatement following these rules:

- **Preserve meaning exactly** — do NOT change what the law says, only HOW it
  says it. The simplified text must be legally equivalent.
- **Use everyday vocabulary** — replace "stanowienie prawa" with "tworzenie
  prawa" only if the meaning is preserved.
- **Unpack cross-references** — instead of "z zastrzeżeniem ust. 2", briefly
  state what ust. 2 says or use "chyba że..." (unless...).
- **Active voice** — prefer "Sejm uchwala ustawy" over "Ustawy są uchwalane
  przez Sejm".
- **Short sentences** — break long provisions into numbered points where
  the original uses semicolons or comma-separated clauses.
- **Both languages** — provide Polish simplified text, then English simplified
  text. The English should be a simplification of the English translation, not
  a translation of the Polish simplification.

### 5. Show the diff

For each article, present the output as a side-by-side comparison:

```
## Art. <N> — <short title>

### Original (Polish)
<verbatim text from docstring>

### Simplified (Polish)
<your plain-language restatement>

### Original (English)
<verbatim text from docstring>

### Simplified (English)
<your plain-language restatement>

### Changes made
- <bullet list of what was simplified and why>
```

### 6. Caveats

At the end, always include this disclaimer:

> **Note:** This simplification is for readability only. It is NOT a proposed
> amendment. The original constitutional text remains the binding version.
> Simplification may inadvertently narrow or broaden meaning — always verify
> against the original.
