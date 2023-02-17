# Setting up a project for git

## Git config

To start you need to setup some configuration variables. To do this we use the `git config` command

```bash
git config --glbal user.name "Your Name"
git config --global user.email "Your Email"
```

The code above sets the name and email configuration variables globally.

## Initializing a project as a git repo

In order to use git in a project, you first need to initialize it as a git repository. To this, you use the command:

```bash
git init
```

Once git is initialized, it can begin tracking all changes that occur in the project.

## Staging Files

The staging environment is a temporary area that we can store files that we want to commit later on. To stage files, use the commands below:

```bash
git add FILENAME
# git add --all
# git add -A
# git add -A
```

The `--all`, `-A` and `.` options all do the same thing – adds all the files in the project.

## Commiting Files

To commit files, we use the commit command

```bash
git commit -m "Initial Commit"
```

We add a `-m` flag and add a commit message to describe what that specific commit did.
