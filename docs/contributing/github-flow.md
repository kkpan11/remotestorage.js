# GitHub workflow

## General guidelines

- When you start working on an existing GitHub issue (or you plan on
  doing that in the immediate future), assign it to yourself, so that
  others can see it and don't start working on it in parallel.
- When you create a branch to work on something, use the naming scheme
  described further down in this document.
- Never push directly to the `master` branch for any changes to the
  source code itself.
- As soon as you want others to review your changes, or even just
  discuss them, create a pull request. Don't forget to explain
  roughly what it is you're doing in that branch, i.e. what the
  problem/idea is and what the result is supposed to be, when merging
  the changes. If necessary or helpful, mention related discussions
  from other issues.
- A pull request can be merged as soon as at least two people with
  commit access to the repo have given a +1, meaning they reviewed and
  tested the changes and have no further improvements to suggest.

## Branch names

Using common branch names, that include topics and issue IDs, makes
everyone's lives much easier, and keep the repo clean. Branches on our
organization repositories should be created using the following scheme:

`[bugfix|feature|docs|refactor]/[issue id]-[description_with_underscores]`

So for example, if you want to work on fixing a bug with let's say
initial sync, that is described in issue #423, the branch should look
something like:

`bugfix/423-race_condition_on_initial_sync`

And if it's an enhancement to the widget it could look like this e.g.:

`feature/321-customizable_widget_content`

If there's no issue yet, create one first!

## Pulling changes

Always use `--rebase` when pulling code from the remote repo. That way
your local changes are added on top of the current history, avoiding
merge commits and mixing up the commit history. You can set up Git to
use rebase on every pull by default by running
`git config --global pull.rebase true` once.

If you also add the `autostash` option, Git will stash any changed files
before the pull and unstash them afterwards:
`git config --global rebase.autoStash true`.

If you don't want to configure both options globally, or you prefer a
catchier command name for updating a repository with remote changes, we
recommend configuring an alias, like so:
`git config --global alias.up 'pull --rebase --autostash'`. Now you can
simply run `git up` in select repos, or everywhere.

## Commit messages

- The first line of the message (aptly called \"subject line\" in Git
  terminology) should not be longer than 72 characters.
- If the subject line is not enough to describe the changes properly,
  add a blank line after the subject line and then as much text as you
  want, using normal language with capitalization, punctuation, etc.
- Always use messages that describe roughly *what* the change does
  and, if not obvious, *why* this change leads to the desired result.
- Leave out any text that isn't directly associated with the changes,
  that the commit introduces. Examples: \"as suggested by
  \@chucknorris\", \"lol wtf was that\", \"not sure if this fixes
  it\".
- Commit as much and often as possible locally (and with any message
  that helps you during your work), and then clean up the history and
  merge commits that belong together before pushing to the org repo.
  You can do that with `git rebase -i [ref]` ([learn
  more](http://www.reviewboard.org/docs/codebase/dev/git/clean-commits/#rewriting-history)).
- You can reference issues from commit messages by adding keywords
  with issue numbers. Certain keywords will even close the issue
  automatically, once a branch is merged into master. For example
  `Fix widget flickering when opening bubble (fixes #423)` will close
  issue #423 when appearing on the master branch at GitHub.

## Reviewing pull requests

- Check if it works, if it has unit tests, if the tests pass, and if
  the linter is happy.
- Check if the code is understandable, with clear and unambiguous
  names for functions and variables, and that it has TypeDoc comments
  and a changelog entry.
- If the pull request was issued from a user's own repository, you
  will have to fetch the code from there. If you haven't pulled from
  their fork previously, you can add a new remote for it with
  `git remote add [username] [repo-url]`. Then, `git fetch [username]`
  will fetch code from this remote, so you can then check out their
  branch using `git checkout [username]/branchname`.
- This will put you in a so-called 'detached HEAD' state, but don't
  worry, everything is fine! If you want to work on that code, just
  create a new branch from there with the command Git tells you then,
  or just go back to your code with e.g. `git checkout master` later.)

## Merging pull requests

- Once a pull request has two +1s for the latest changes from
  collaborators, you can either merge it yourself or wait for somebody
  to do it for you (which will happen very soon).
- If the new commits and their commit messages in that branch all make
  sense on their own, you can use the merge button on GitHub directly.
- If there are a lot of small commits, which might not make sense on
  their own, or pollute the main project history (often the case with
  long running pull requests with a lot of additions during their
  lifetime), fetch the latest changes to your local machine, and
  either do an interactive rebase to clean up branch and merge
  normally, or use `git merge --squash` to squash them all into one
  commit during the merge.
- Whenever you squash multiple commits with either `git rebase -i` or
  `git merge --squash`, make sure to follow the commit message
  guidelines above. Don't just leave all old commit messages in there
  (which is the default), but delete them and create a new meaningful
  message for the whole changeset.
- When squashing/editing/amending other peoples' commits, use
  `--author` to set them as the original author. You don't need full
  names for that, but just something that Git can find in the history.
  It'll tell you if it can't find an author and let you do it again.
