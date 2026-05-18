`README.md` is the canonical index of all skills in this repository.

**Whenever you add, rename, or remove a skill, you must update `README.md`.**

### Rules

- Every skill lives in `skills/<skill-name>/SKILL.md` and has YAML frontmatter with `name` and `description` fields.
- The README skills table must stay in sync: one row per skill, sorted alphabetically by name.
- Pull the description from the `description` field in the skill's frontmatter — and summarize it. 
- Table format:

  ```markdown
  | Skill | Description |
  |---|---|
  | [skill-name](skills/skill-name/SKILL.md) | …description… |
  ```

- Do not add any row for a skill that has no `SKILL.md`.
- Remove the row when a skill folder is deleted.
