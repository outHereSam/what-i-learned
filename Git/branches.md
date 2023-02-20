# Branches

Branches let you create different versions of your code/project so you can play around with or add new features without messing up the main code branch.
To view the current working branches, use the command:

```bash
git branch
# * main
```

To copy an existing branch and create a new branch we use the command:

```bash
git switch -c NAME
# git checkout -b NAME
```

## Merging

The merge command will merge a branch into the current branch

```bash
git merge <branch>
```

## Deleting a branch

When you're done working on a branch and merging it into the main branch, it's a good idea to delete that branch. To do that we use the `delete` command:

```bash
git branch --delete NAME
# git branch -d NAME
# git branch -D NAME
```
