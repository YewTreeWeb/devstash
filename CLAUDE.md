@AGENTS.md

## Code quality skills

- When writing or editing code, load the `antislop-code`, `antislop-human`, and `modern-web-guidance` skills before you start.
- When reviewing code (a diff, a PR, or a file), load the same three skills and apply their rules as review criteria.
- When reviewing code, also run the `caveman:caveman-review`, `ponytail:ponytail-review`, `fallow-review`, and `code-review` skills.
- Show the user every finding from these review skills, merged into one list. Label every finding with the skill that found it, e.g. `[ponytail-review]`. When two or more skills report the same issue (same location and same problem), show it once with every reporting skill in the label, e.g. `[caveman-review, ponytail-review]`.
