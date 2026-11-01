`README.md` is the canonical index of all skills in this repository.

**Whenever you add, rename, or remove a skill, you must update `README.md`.**

### Rules

- Every skill lives in `skills/<skill-name>/SKILL.md` or `skills/<category>/<skill-name>/SKILL.md` and has YAML frontmatter with `name` and `description` fields.
- Group skills in `README.md` by their category folder. Each category heading must include a short explanation of the group, followed by one bullet per skill.
- Sort skill bullets alphabetically by name within each category. Links must point to the skill's actual `SKILL.md` path.
- Pull each summary from the `description` field in the skill's frontmatter.
- List format:

  ```markdown
  ### category

  Short explanation of this skill group.

  - [skill-name](skills/category/skill-name/SKILL.md) — …description…
  ```

- Do not add a bullet for a skill that has no `SKILL.md`.
- Remove the bullet when a skill folder is deleted.
