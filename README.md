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

We can add new prefixes such as `wip: […]` and have standard-version ignore
those when making the changelog. Any such list needs to be compiled first…

## Commit messages format

[Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) is the
main source of information, but as an aid mémoire:

+ **feat**: a new feature
+ **fix**: a bug fix
+ **docs**: documentation only change
+ **style**: changes that do not affect the meaning of the code -- formatting
+ **refactor**: a code change that neither fixes a bug nor adds a feature
+ **perf**: A code change that improves performance
+ **test**: Adding a missing test
+ **chore**: Changes to the build process or auxiliary tools and libraries such as
  documentation generation

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

## NPM

### Install npm packages globally without sudo on macOS and Linux

Run:

```bash
mkdir "${HOME}/.npm-packages"
npm config set prefix "${HOME}/.npm-packages"
```

Then update `~/.bashrc` or `.zshrc` with:

```
NPM_PACKAGES="${HOME}/.npm-packages"
export PATH="$PATH:$NPM_PACKAGES/bin"
# Preserve MANPATH if you already defined it somewhere in your config.
# Otherwise, fall back to `manpath` so we can inherit from `/etc/manpath`.
export MANPATH="${MANPATH-$(manpath)}:$NPM_PACKAGES/share/man"
```

More details on the [Install npm packages globally without sudo on macOS and
Linux](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md)
page.

### conventional-changelog-cli

From the
[GitHun page](
https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli):

```bash
npm install -g conventional-changelog-cli
conventional-changelog -p angular -i CHANGELOG.md -s
```

## Push requests

### PR merge

This is a basic merge…

*conventional-changelog-cli seems to ignore this type of PR.*

*standard-version*

### PR squash

This is a basic squash then merge…

*conventional-changelog-cli seems to ignore this type of PR.*

### PR rebase

This is a basic rebase then merge…

This looks like it is going to be needed if we wanted to use the
conventional changelog methodology. All the commits are rebased thus an
additional step of cleaning up the local branch before a merge needs to
happen. For example, `wip: blah blah…` messages need to be squashed into one
commit -- the final one. It might be possible to excludes certain prefixes,
although I am not sure how.

The rule is: *The angular commit schema is best for perfect commits.*

[standard-version](https://github.com/conventional-changelog/standard-version)
might be a better tool for ignoring specific commits.

### Alternative

There is
[github-changelog-generator](https://github.com/github-changelog-generator/github-changelog-generator)
which works specifically for GitHub PRs and can be called from docker like so:

```bash
docker run -it --rm -v "$(pwd)":/usr/local/src/your-app ferrarimarco/github-changelog-generator -u worldr -p test-CI -t $CHANGELOG_GITHUB_TOKEN
```

[This is the changelog it generated](CHANGELOG_GITHUB_GENERATOR.md).

## Release

### [Standard version](https://github.com/conventional-changelog/standard-version)

This does:

1. Retrieve the current version of your repository by looking at
   packageFiles, falling back to the last git tag.
1. bump the version in bumpFiles based on your commits.
1. Generates a changelog based on your commits (uses conventional-changelog
   under the hood).
1. Creates a new commit including your bumpFiles and updated CHANGELOG.
1. Creates a new tag with the new version number.

Commands:

```bash
npx standard-version --dry-run --prerelease RC --no-verify # Create a RC release.
npx standard-version --dry-run --no-verify # Create a normal release.
npx standard-version --help # Err, help?
```

`--no-verify` stops the pre-commit hook from running.

[How is standard-version different from
semantic-release?](https://github.com/conventional-changelog/standard-version#how-is-standard-version-different-from-semantic-release)
is  good read.

### [Semantic release](https://github.com/semantic-release/semantic-release)

This does:

1. **Verify Conditions**	Verify all the conditions to proceed with the
   release.
1. **Get last release**	Obtain the commit corresponding to the last release by
   analyzing Git tags.
1. **Analyze commits**	Determine the type of release based on the commits
   added since the last release.
1. **Verify release**	Verify the release conformity.
1. **Generate notes**	Generate release notes for the commits added since the
   last release.
1. **Create Git tag**	Create a Git tag corresponding to the new release
   version.
1. **Prepare**	Prepare the release.
1. **Publish**	Publish the release.
1. **Notify**	Notify of new releases or errors.

[Full documentation is
here](https://semantic-release.gitbook.io/semantic-release/) and it is good™.

However, this is a beast and very much linked to NodeJS and automate the whole
process. This is a good thing™ but does not fit our needs just yet…
