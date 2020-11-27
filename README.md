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

## gitlint

Please install [gitlint](https://github.com/jorisroovers/gitlint) as shown
[here](https://jorisroovers.com/gitlint/#getting-started). The pre-commit hook
should take care of it all but if you do not want to use that, please [follow
those
steps](https://jorisroovers.com/gitlint/#using-gitlint-as-a-commit-msg-hook).

## Do release

### Town crier: Hear ye! Head ye!

The main [towncrier](https://github.com/twisted/towncrier) page.

To create a new entry, use this:

```bash
towncrier create slug.type
git add changes/slug.type
```
where

+ `slug` is an arbitrary slug referring to a YT issue, epic, or whatever internal
  specific information we want.
+ `type` is one of "feature", "bugfix", "doc", "removal", or "misc".

This creates an
[rst](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html)
formatted file (but without the suffix) which will be added to the `NEWS.rst`
file.

This file need to be added to git.

To see what the new release will look like:

```bash
towncrier build --draft --name release-name --version release-sem-ver
```

Where  `release-name` is a name for the release and `release-sem-ver` is the
release version. This will print to `stdout` a rst formatted fragment which
gets added to `NEWS.rst` on release.

To do a release:
```bash
towncrier build --name release-name --version release-sem-ver
```

Note that this *will remove all the files under `./changes`*. You have been
warned!
