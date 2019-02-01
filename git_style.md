## Git styling

### Basic
**1.** If you start working on project form scratch, Create a Git repository for it.

**2.** Set git global user name and email blank. Use below git command.

```git
git config --global user.name ""
git config --global user.email ""
```
**3.** Always set git user name and email, for each project before start working.

```git
git config user.name "RequiredName"
git config user.email "RequiredEmail"
```

**4.** Create a new branch for every new feature.

**5.** Use Pull Request to merge code to Master.

**6.** Never push changes directly to the master branch.

**7.** Use `gitk` to see the changes and commits (Install gitk, if not installed)

```git
sudo apt-get install gitk
```

gitk provide GUI for git

### Branches

**1** Always use _short_ and _descriptive_ branch names.

```git
# bad - too vague
$ git checkout -b login_fix

# good
$ git checkout -b oauth-migration
```

**2.** Identifiers from related tickets in an external service (eg. task number from PM tools, Git issues etc) are also good to use in branch names.

```git
# GitHub issue #15
$ git checkout -b issue-15
```

**3.** Use _hyphens_(-) to separate words instead of underscore(_).
```git
# bad
$ git checkout -b login_fix

# good
$ git checkout -b oauth-migration
```

**4.** When several people are working on the _same_ feature, it might be convenient to have _personal_ feature branches and a _team-wide_ feature branch. Use the following naming convention.

```git
$ git checkout -b feature-a/master # team-wide branch
$ git checkout -b feature-a/maria  # Maria's personal branch
$ git checkout -b feature-a/nick   # Nick's personal branch
```

**5.** Delete your branch from the upstream repository after it's merged, unless there is a specific reason not to delete.

**6.** Test before you push. Do not push half-done work.

### Commits

**1.** Each commit should be a single _logical change_. Don't make several _logical changes_ in one commit. For example, if a patch fixes a bug and optimizes the performance of a feature, split it into two separate commits.

In below example first commit include both different task in single commit. So it should be in separate one.

```git
#bad
$ git commit -m 'patch fixes a bug and optimizes the performance of feature'

#good
$ git commit -m 'Patch fixes a bug'
$ git commit -m 'Optimizes the performance of feature'
```

**2.** Don't split a single _logical change_ into several commits. For example, the implementation of a feature and the corresponding tests should be in the same commit.

**3.** Commit _early_ and _often_. Small, self-contained commits are easier to understand and revert when something goes wrong.

**4.** Commits should be ordered _logically_. For example, if _commit X_ depends on changes done in _commit Y_, then commit Y should come before commit X.

 **5.**  Never do `git reset --hard`
 ```git
#bad
git reset --hard HEAD

#good
git reset --soft HEAD
 ```

 **7.** Rebase instead of merge if you are only the developer working on project.
```git
git  rebase master
```
**8.** Never use rebase on public branch(branch which code from multiple developer) if multiple developer working on project.

**9.** The golden rule of `git rebase` is to never use it on _public_ branches

**10.** Don't commit configuration files.  like files which contain the passwords, secret keys etc.

**11.** Don't commit large binary files. like database dump, large zip files etc.

### Messages

**1.** Use the editor, not the terminal, when writing a commit message.

```git
# bad
$ git commit -m "Quick fix"

# good
$ git commit
```

**2.** The summary line (ie. the first line of the message) should be descriptive with brief.

**3.** The normal git rule of using the first line to provide a short summary of the change.

**4.** Commit message first line should be no longer than 50 characters.

**5.** Message should be capitalized and written in imperative present tense.

```git
# bad
fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails

#good
Mark huge records as obsolete when clearing hinting faults
```

**6.** Message should not end with a period since it is effectively the commit title.

```git
# bad
fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.

# good - imperative present tense, capitalized, fewer than 50 characters
Mark huge records as obsolete when clearing hinting faults
```

### Merging

**1.** Don't try to change the repository's history. It is very important to be able to tell _what actually happened_. Altering published history is a common source of problems for anyone working on the project.

**2.** However, there are cases where rewriting history is ok. These are when:

-   You are the only one working on the branch and it is not being reviewed.

-   You want to tidy up your branch (eg. squash commits) and/or rebase it onto the "master" in order to merge it later.

**3.** If your branch includes more than one commit, do not merge with a fast-forward.

```git
# bad
$ git merge my-branch

# good - ensures that a merge commit is created
$ git merge --no-ff my-branch
```

**4.** Always name your stashes.

```git
git stash save 'name stash'
```

**5.** Don't use `reset` (`--hard | --merge`) without committing/stashing.
