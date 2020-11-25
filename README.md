# test-CI

Testing filters for CircleCI

## Develop branch

That is the unstable one.

Sean's change from fix/test plus direct to master

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

