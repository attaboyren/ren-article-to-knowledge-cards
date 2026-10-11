# make-knowledge-cards

Turn a pasted article or local text file into focused knowledge cards for study and review. This project is an instruction-only Codex skill: Codex reads the instructions and creates the cards. There is no standalone converter or application to run.

## Problem it solves

Articles can bury important ideas in repeated wording, supporting details, and anecdotes. This skill guides Codex to select the most useful knowledge and turn it into concise cards, with one concept per card and no added information beyond the source.

## Main features

- Accepts pasted article text or a local `.md`, `.markdown`, or `.txt` file.
- Produces 5-8 cards when the source supports enough distinct concepts; fewer for sparse sources, and none when there is no substantive knowledge.
- Gives each card a title, key concept, brief explanation, and either a source example or a self-test question with an answer.
- Prioritizes central ideas, merges duplicates, and keeps one concept per card.
- Preserves qualifications, attribution, numbers, and unresolved disagreements. Examples and self-test answers must be supported by the source.
- Returns Markdown cards in the conversation, using the source's language unless another language is requested.

Web scraping, URL fetching, PDF input, Anki export, and a graphical interface are not supported. Instructions embedded in an article are treated as source content rather than commands.

## Installation

You need Codex with access to the source text or local file. Using this skill requires no Python environment, package installation, or build step.

1. Download or copy this project's files to your computer.
2. Copy the entire `skills/make-knowledge-cards` folder into `.agents/skills/` in the project where you want to use it. The installed file should be at `.agents/skills/make-knowledge-cards/SKILL.md`.
3. If the skill does not appear in Codex, restart Codex.

For example, run these PowerShell commands from this project's root to install it here:

```powershell
New-Item -ItemType Directory -Path .agents/skills -Force | Out-Null
Copy-Item -LiteralPath skills/make-knowledge-cards -Destination .agents/skills -Recurse
```

Codex discovers local skills under `.agents/skills/`; see the [official skill documentation](https://learn.chatgpt.com/docs/build-skills). The authoring copy remains under `skills/`. You can also use that copy directly with the explicit-loading prompt below.

## Usage

Once installed, invoke the skill with pasted text:

```text
Use $make-knowledge-cards on this article:
[paste your article here]
```

Or supply a local file path:

```text
Use $make-knowledge-cards to make cards from tests/inputs/procedure.md.
```

To try the authoring copy from this project's root without installing it:

```text
Read and follow skills/make-knowledge-cards/SKILL.md.
Make knowledge cards from tests/inputs/procedure.md.
```

Replace the sample path with your own readable Markdown/TXT file. To save the result, include a request such as `Save the cards to workshop-cards.md`; otherwise, the cards are returned in the conversation.

## Sample inputs and outputs

These complete examples use the retained local test fixtures:

| Article | Sample input | Sample output |
| --- | --- | --- |
| Workshop procedure | [Markdown article](tests/inputs/procedure.md) | [Six cards](tests/outputs/procedure.md) |
| Fictional classroom study | [TXT article](tests/inputs/study.txt) | [Five cards](tests/outputs/study.md) |

A short, repetitive source produces only one card. Input:

```text
A checksum is a value used to check whether data has changed.
Put another way, a checksum helps check for changes in data.
```

Output:

```markdown
The text supports only one distinct concept, so there is one card.

### 1. Checksum
**Key concept:** A checksum is a value used to check whether data has changed.
**Explanation:** The article repeats this same definition in different words.
**Self-test:** What is a checksum used to check? Answer: Whether data has changed.
```

The repeated definition is merged, and a self-test is used because the source supplies no example. Exact wording may vary between runs.

## Development and validation

The skill was created with the official `skill-creator/scripts/init_skill.py`. Its package contains only `SKILL.md` and `agents/openai.yaml`.

[Behavioral tests](tests/behavioral-tests.md) retain eight local manual trials, their outputs, review criteria, input checks, and the resulting instruction improvements. To retest, load the skill and apply it to those sources, checking meaning against the originals. Wording may vary.

Python and PyYAML are needed only for the optional development validator. Replace the placeholder with your local skill-creator installation path:

```text
python "<path-to-skill-creator>/scripts/quick_validate.py" skills/make-knowledge-cards
```

Structural validation passed. Behavioral testing was performed by the authoring agent, without an independent model evaluation.

## License

[MIT](LICENSE).
