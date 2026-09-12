# Contributing

Each public skill belongs in its own top-level directory:

```text
skill-name/
├── SKILL.md
└── referenced-assets...
```

Keep `SKILL.md` focused on a single user outcome. Reference supporting files directly from it and explain when an agent should read each one.

Before proposing a change, confirm that it preserves the skill's stated scope, has no unnecessary runtime dependency, and leaves its examples usable as standalone artifacts.

## Release checklist

1. Confirm every referenced local file exists.
2. Open and read any HTML example in a browser.
3. Check the install path locally:

   ```sh
   npx skills add https://github.com/Bishoymly/skills --skill <skill-name>
   ```

4. Update the root README when a skill is added or materially changed.
5. Commit and push the release to `main`.
