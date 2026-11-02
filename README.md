# Code Review Skill

An AI agent skill for comprehensive, three-axis code reviews evaluated across parallel sub-agents to avoid context pollution.

## Overview

The `code-review` skill examines changes between `HEAD` and a specified fixed point (e.g., `main`, `HEAD~1`, commit SHA, or tag) along three distinct axes:

1. **Standards** — Conformance to repository-documented coding conventions and a baseline of Martin Fowler code smells.
2. **Spec** — Fidelity to the originating issue description or specification document, catching missing requirements and scope creep.
3. **Complexity** — Algorithmic Big-O time and peak auxiliary-space analysis before and after the change.

By running each evaluation axis in independent, parallel sub-agents, evaluations remain unpolluted and objective. Findings are aggregated side-by-side without reranking or masking.

## Key Features

- **Standards with Fowler Smell Baseline**: Checks repo standards (`CODING_STANDARDS.md`, `CONTRIBUTING.md`) and layers a 12-smell baseline (*Mysterious Name*, *Duplicated Code*, *Feature Envy*, *Primitive Obsession*, etc.) when no documented rules exist. Repo rules always take precedence.
- **Spec Matching**: Automatically locates specs from commit references (`#123`, `Closes #45`), arguments, or repo doc paths, highlighting missing features or scope creep. Skips cleanly if no spec exists.
- **Big-O Complexity Analysis**: Evaluates algorithmic paths against the merge-base baseline for worst-case, expected, or amortized time and peak auxiliary space before/after. Retains full path assessments without arbitrary truncation.

## Installation

Install using [skills.sh](https://skills.sh) / Skills CLI:

```bash
npx skills add PedrocaODev/code-review
```

Preview without installing:

```bash
npx skills add PedrocaODev/code-review -l
```

## Usage

Invoke the skill within your AI agent environment (Claude Code, Antigravity, Cursor, etc.):

```bash
/code-review <fixed-point>
```

### Examples

- Review changes against `main`:
  ```bash
  /code-review main
  ```
- Review the latest commit:
  ```bash
  /code-review HEAD~1
  ```
- Review against a specific commit SHA or tag:
  ```bash
  /code-review a1b2c3d
  /code-review v1.2.0
  ```

## Project Structure

```
.
├── .gitignore
├── LICENSE
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

## License

This project is licensed under the [MIT License](LICENSE).
