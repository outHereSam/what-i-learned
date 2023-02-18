# Understanding Git environments

## Git Information

To get information about a commit, we use the `git log` command, it should show you something like this:

```bash
commit 96455d9c1f635759d68af7452982810e7408111f (HEAD -> main)
Author: outHereSam <'Email goes here'>
Date:   Fri Feb 17 23:02:05 2023 +0000

    Learns how to set up git in a project
```

## Git Environments

- **Working**
  This environment contains files that look like how they did before the last commit.

- **Staging**
  We add files to the staging environment with the `git add` command. This allows us to queue up changes before commiting them, kinda like dating before marriage.

- **Commit**
  Once you commit changes, a new log entry is created with a new hash.

## File States

Files being tracked by git can be in one of two states:

- Tracked
  These are files that existed in the previous snapshot, which is another name for the commit that you did.
  Tracked files can also be in one of three states:

  - Unmodified: The files haven't changed since the last commit
  - Modified: The files have changed since the last commit
  - Staged: The files have been moved to the staging area.

- Untracked
  These are new files added before the last commit.

You can examine what happened with the `git status` command

## Restoring Files

To discard changes in the working directory, use the `git restore` command. You can issue a git restore with a file name or a period (`.`), to restore the whole directory.
To restore files from the staging directory back into the working directory, use the `git restore --staged` command which can be shortened as `git restore -S`
