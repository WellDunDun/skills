---
name: bro
description: Restate the last agent or assistant message in plain human language, with no jargon. Use when the user says "bro", asks for a simpler restatement, or says an answer is verbose, confusing, incoherent, too technical, or hard to parse.
disable-model-invocation: true
---

# Bro

Restate the previous assistant message so the user can understand it quickly.

## Response Rules

1. Say the same thing in plain human language.
2. Keep only facts, decisions, blockers, commands, file paths, and next steps the user needs.
3. Use short sentences and normal words.
4. Replace jargon with concrete meaning. If a technical term is necessary, define it in the sentence.
5. Do not add new analysis, warnings, alternatives, or work that was not in the previous message.
6. Do not apologize for the previous wording unless the user asked for an apology.
7. Keep simple restatements to one short paragraph. Use 3-6 bullets only when the original message contains several separate items.
8. If the previous message asked a question, ask the same question more plainly.
9. Preserve exact commands, code identifiers, file paths, URLs, error text, and status names.

## Output Shape

Start directly with the clearer version. Do not preface it with phrases like "In simple terms" or "What I meant was."
