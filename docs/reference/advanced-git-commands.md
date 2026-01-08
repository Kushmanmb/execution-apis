# Advanced Git Commands

This guide covers advanced Git commands that are useful when working with this repository. These commands will help you manage your work more effectively, handle complex scenarios, and maintain a clean commit history.

## git stash

The `git stash` command temporarily saves changes you've made to your working directory so you can work on something else, and then come back and re-apply them later.

### Basic Usage

```console
$ git stash
Saved working directory and index state WIP on main: 5c3e2b1 Update documentation
```

This command saves your local modifications and reverts the working directory to match the HEAD commit.

### Common Stash Operations

**List all stashes:**
```console
$ git stash list
stash@{0}: WIP on main: 5c3e2b1 Update documentation
stash@{1}: WIP on feature-branch: 3a2b1c4 Add new method
```

**Apply the most recent stash:**
```console
$ git stash apply
```

**Apply a specific stash:**
```console
$ git stash apply stash@{1}
```

**Apply and remove the stash:**
```console
$ git stash pop
```

**Save stash with a descriptive message:**
```console
$ git stash save "WIP: implementing new API method"
```

**Stash including untracked files:**
```console
$ git stash -u
```

**View stash changes:**
```console
$ git stash show -p stash@{0}
```

**Delete a specific stash:**
```console
$ git stash drop stash@{0}
```

**Clear all stashes:**
```console
$ git stash clear
```

### Use Cases

- You need to quickly switch branches but have uncommitted changes
- You want to temporarily set aside work to test something else
- You need to pull latest changes but have local modifications

## git cherry-pick

The `git cherry-pick` command applies the changes introduced by existing commits to your current branch. This is useful when you want to apply specific commits from one branch to another without merging the entire branch.

### Basic Usage

```console
$ git cherry-pick 3a2b1c4
[main 8f7e6d5] Add validation for new parameter
 Date: Mon Jan 8 10:30:00 2024 +0000
 1 file changed, 5 insertions(+)
```

This applies the changes from commit `3a2b1c4` to your current branch.

### Cherry-picking Multiple Commits

**Cherry-pick a range of commits:**
```console
$ git cherry-pick A^..B
```

This picks all commits from A (exclusive) to B (inclusive).

**Cherry-pick multiple specific commits:**
```console
$ git cherry-pick 3a2b1c4 5d6e7f8 9g0h1i2
```

**Cherry-pick without committing:**
```console
$ git cherry-pick -n 3a2b1c4
```

This applies the changes but doesn't create a commit, allowing you to modify the changes before committing.

### Handling Conflicts

If cherry-picking results in conflicts:

```console
$ git cherry-pick 3a2b1c4
error: could not apply 3a2b1c4... Add new feature
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```

Resolve conflicts, then:
```console
$ git add <resolved-files>
$ git cherry-pick --continue
```

**Abort cherry-pick:**
```console
$ git cherry-pick --abort
```

### Use Cases

- Applying a bug fix from one branch to another
- Backporting features to a release branch
- Moving commits between branches selectively

## git revert

The `git revert` command creates a new commit that undoes the changes from a previous commit. Unlike `git reset`, it doesn't alter the existing commit history, making it safe for use on shared branches.

### Basic Usage

```console
$ git revert 3a2b1c4
[main 9h8i7j6] Revert "Add problematic feature"
 1 file changed, 10 deletions(-)
```

This creates a new commit that reverses the changes introduced by commit `3a2b1c4`.

### Reverting Multiple Commits

**Revert a range of commits:**
```console
$ git revert A^..B
```

**Revert without auto-committing:**
```console
$ git revert -n 3a2b1c4
```

This applies the revert but doesn't commit, allowing you to revert multiple commits in a single commit:

```console
$ git revert -n 3a2b1c4
$ git revert -n 5d6e7f8
$ git commit -m "Revert problematic changes"
```

**Revert a merge commit:**
```console
$ git revert -m 1 <merge-commit-sha>
```

The `-m 1` option specifies that you want to revert to the first parent (usually the main branch before the merge).

### Handling Conflicts

If reverting causes conflicts:

```console
$ git revert 3a2b1c4
error: could not revert 3a2b1c4... Add feature
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git revert --continue'
```

Resolve conflicts, then:
```console
$ git add <resolved-files>
$ git revert --continue
```

**Abort revert:**
```console
$ git revert --abort
```

### Use Cases

- Undoing a commit that has already been pushed to a shared branch
- Safely removing problematic changes without rewriting history
- Creating an audit trail of what was undone and why

## git reset

The `git reset` command resets your current HEAD to a specified state. It can be used to undo commits, unstage files, or discard changes. **Warning:** This command can modify history and should be used carefully, especially on shared branches.

### Types of Reset

**Soft reset (--soft):**
```console
$ git reset --soft HEAD~1
```

Moves HEAD back one commit but keeps changes in the staging area. Useful for amending commits or re-organizing commits.

**Mixed reset (--mixed, default):**
```console
$ git reset HEAD~1
```

Moves HEAD back one commit and unstages changes, but keeps them in the working directory.

**Hard reset (--hard):**
```console
$ git reset --hard HEAD~1
```

Moves HEAD back one commit and **discards all changes**. Use with extreme caution!

### Common Reset Operations

**Unstage a file:**
```console
$ git reset HEAD file.txt
```

**Unstage all files:**
```console
$ git reset HEAD
```

**Reset to a specific commit:**
```console
$ git reset --hard 3a2b1c4
```

**Reset to remote branch state:**
```console
$ git reset --hard origin/main
```

**Keep changes but undo last commit:**
```console
$ git reset --soft HEAD~1
```

### Reset vs Revert

| Feature | reset | revert |
|---------|-------|--------|
| Alters history | Yes | No |
| Safe for shared branches | No | Yes |
| Creates new commit | No | Yes |
| Can undo multiple commits | Yes | Yes |
| Can unstage files | Yes | No |

### Use Cases

- Undoing local commits before pushing
- Unstaging files accidentally added to staging area
- Discarding local changes to match remote branch
- Combining multiple commits into one (soft reset, then commit)

### Important Warnings

⚠️ **Never use `git reset --hard` on commits that have been pushed to a shared branch**, as this will rewrite history and cause problems for other contributors.

⚠️ **Always double-check before using `--hard`**, as it permanently discards changes.

## Best Practices

### When to Use Each Command

- **Use `git stash`** when you need to temporarily set aside work
- **Use `git cherry-pick`** when you need specific commits from another branch
- **Use `git revert`** when you need to undo commits on shared branches
- **Use `git reset`** when you need to undo local commits that haven't been pushed

### Workflow Tips

1. **Before switching branches unexpectedly:**
   ```console
   $ git stash
   $ git checkout other-branch
   # Do work
   $ git checkout original-branch
   $ git stash pop
   ```

2. **Fixing a commit message (local only):**
   ```console
   $ git reset --soft HEAD~1
   $ git commit -m "Better commit message"
   ```

3. **Applying a bug fix to multiple branches:**
   ```console
   $ git checkout main
   $ git cherry-pick <bug-fix-commit>
   $ git checkout release-branch
   $ git cherry-pick <bug-fix-commit>
   ```

4. **Undoing a pushed commit safely:**
   ```console
   $ git revert <commit-sha>
   $ git push
   ```

## Additional Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [Git Book - Advanced Topics](https://git-scm.com/book/en/v2)
- [Contributors Guide](contributors-guide.md) - for contributing to this repository
