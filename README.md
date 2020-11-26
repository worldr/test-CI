# test-CI

Testing filters for CircleCI and releases procedures…

VERSION 0.0.0

## Develop branch

That is the unstable one.

Sean's change from fix/test plus direct to main.

## Main branch is now "main", not master

If you have a checkout, please use this:

```bash
# Switch to the "master" branch:
$ git checkout master

# Rename it to "main":
$ git branch -m master main

# Get the latest commits (and branches!) from the remote:
$ git fetch

# Remove the existing tracking connection with "origin/master":
$ git branch --unset-upstream

# Create a new tracking connection with the new "origin/main" branch:
$ git branch -u origin/main
```

## Pre commit

This uses [pre-commit](https://pre-commit.com/). Thus, you should run:

```bash
python -m pip install pre-commit
pre-commit install
pre-commit autoupdate  # Only if you want the latest versions!
```

Then, every time you commit, the commit will be checked for all the things
present in the `.pre-commit-config.yaml` configuration file.

### gitlint

Please install [gitlint](https://github.com/jorisroovers/gitlint) as shown
[here](https://jorisroovers.com/gitlint/#getting-started). The pre-commit hook
should take care of it all but if you do not want to use that, please [follow
those
steps](https://jorisroovers.com/gitlint/#using-gitlint-as-a-commit-msg-hook).
