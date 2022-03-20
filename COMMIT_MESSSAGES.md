# Semantic Versioning
Most projects will be versioned using a three part numeric code
like `22.6.37`. The first part is the major version number, the
second part the minor version number. The final part is the
patch number.

Generally when the patch version is incremented, there have been
bug fixes but no breaking changes. When the minor version is
updates, there have been bug fixes with breaking changes and/or
small features and enhancements. When the major version changes
there have been major features and enhancements.

# Commit Messages
A brief description of Conventional Commits can be found at
[this gist](https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716).
A more thorough treatment is at
[ConventionalCommits](https://www.conventionalcommits.org/en/v1.0.0/)

## Short and Sweet
Basically, every commit message should have a prefix:

- A bug fix should be committed like `fix: Corrected the bug where ....`
If the bug fix results in a breaking change, the prefix should be
`fix!: ...`

- A feature should be prefixed with `feat: ...` or `feat!: ...`
for a feature that is not backwards compatible. 

- `chore: ...` is used for a change that does not impact the user code
at all, like changes to config for automatic deployment or testing.

- Other common commit prefixes are `test:...`, `docs: ...`, `style: ...`
and `refactor: ...`.

For a complex project, these commit messages can be scoped to a sub-project
with parenthesis like `fix(config_subsystem): ...`.

# Pull Requests
In most cases changes will be made by making commits on a branch, and then
merging a PR (pull request) to the default branch - `main` or `master`. In these cases
the changes in the pull request will be squashed and merged into the default
branch as a single commit. Since the PR is merged as one single commit It will
make more sense to keep a pull request focused on a single fix, feature, or
other change rather than lumping together a lot of unrelated work in one PR.
The PR should be named using a conventional commit message.

# Release Please
We use a github action called
[release-please](https://github.com/google-github-actions/release-please-action)
that automates the creation of releases.

**release-please** will use the commit messages to automatically figure
out the next version number. A `fix:` will increment the patch release.
A `fix!: ...` or `feat: ...` will increment the minor version number and
set the patch back to 0. A `feat!: ...` will increment the major version
number and reset the minor and patch versions to 0. **release-please** will
use the commit messages to generate a `CHANGELOG.md` file. It will also
create a tag in the repo for each release. If the project is published to
Maven, Rubygems, npmjs.org, etc. release-please can also handle that.

## How It Works
Whenever code is merged with the default branch - `main` or `master` - the
**release-please** action is run. It will check if there is an open PR with
a name like `chore: release main`. If this PR does not exist, it will be created.
If it does exist, it will be updated. This PR will contain a summary
of all the work that is pending release, as well as the `CHANGELOG.md`, the
pending version numbers, etc.

When it is time to actually release the code, an admin will merge that PR. That
triggers **release-please** to actually make the release, tag the repo,
merge the CHANGELOG.md into the default branch, and potentially ship code
to repositories like Maven, RubGems, NPMJS, etc.
