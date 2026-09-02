# Contributing

## How to understand this project? (Q/A)

---

**Q: WHY and HOW this project works (high-level ideas)?**

**A:** [ARCHITECTURE.md](ARCHITECTURE.md) gives the overall idea (_not necessarily technical_)

---

**Q: JUST show the project LAYOUT.**

**A:** [workspace-members.md](docs/dev/workspace-members.md) shows what each crate / package does.

---

**Q: Other concepts?**

**A:** Read the [developer documentation](docs/dev/README.md) Table of Content.

---

**Q: I don't understand anything / didn't find what I wanted, your documentation sucks...**

**A:** Sorry for that, I really tried to make it as understandable as possible. Open issue or email me
to [dreaght@icloud.com](mailto:dreaght@icloud.com) with your question and/or suggestion for documentation improvements.

---

Continue reading for patching / maintenance guidelines.

The key words:

- "**MUST**", "**MUST NOT**", "**REQUIRED**",
- "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**",
- "**RECOMMENDED**",  "**MAY**", and "**OPTIONAL**"

in this document are to be interpreted as described in
[RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

---

## How to commit properly?

- Your commits **MUST** be atomic.
- On changes, you **MAY** release a new artifact. On release:
    - You **MUST** bump [Cargo.toml](Cargo.toml)'s `version`.
    - You **MUST** adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
      for every `version` change.
    - You **MUST** update [CHANGELOG.md](CHANGELOG.md). You **SHALL NOT** dump `git log` or any artificially generated
      content there.

## How to format commits?

- You **MAY** use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#specification).
- You **MAY** use an [emoji guide](https://gitmoji.dev/) for your commit messages.
- Commits **SHALL** be human-readable.

---

## Methodology / Guidelines

- You **SHOULD** adhere to **Test-Driven-Development (TDD)** methodology.
- You **SHALL NOT** use **AI-Driven-Development (AIDD)**, including, but not limited to, **AI Spec-Driven-Development (
  SDD)**. See [AI Policy](docs/dev/ai-policy.md) for more details.

### Command Line Interface (CLI)

- You **SHOULD** adhere to [CLI Guidelines](https://clig.dev/#guidelines).

### External API usage

- You **MUST** adhere to [MediaWiki API Guidelines](https://www.mediawiki.org/wiki/API:Etiquette).
- Read more [about Parsoid](docs/dev/parsoid.md).

### Maintaining Cargo.toml

Read [cargo-toml.md](docs/dev/cargo-toml.md) guidelines.
