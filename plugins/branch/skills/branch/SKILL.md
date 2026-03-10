---
name: branch
description: Create a new git branch and corresponding planning directory.
---

Create a new git branch and corresponding planning directory.

1. Create and checkout a new git branch named: `$ARGUMENTS`
2. Create the planning directory at `./plans/` where the directory name is the branch name with all `/` replaced by `-`

For example, if the argument is `chore/test123`:
- Run: `git checkout -b chore/test123`
- Create directory: `./plans/chore-test123/`

Do both steps, then confirm what was created.
