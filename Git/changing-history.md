# Changing History

To change a previous commit, we use the amend command like so:

```bash
git commit ---amend
```

This lets you replace a commit with a new change.

## Resetting changes

To undo some changes or move back in time to before a change, you can use the `git reset` with the hash of the commit you want to reset to.

## Rebasing

Rebasing is another way of changing history, it takes the commit of one branch and applies it to another branch.

```bash
git rebase <branch>/<commit>
```
