# Github #

- Commit message in terminal to merge

```
press "i" (i for insert)
write your merge message
press "esc" (escape)
write ":wq" (write & quit)
then press enter
```

### Commits

- Ammend last commit 

```
git commit --amend -m "modified commit message" (amend message)
git push --progress origin --force (force push!)
```

### Branches

- ```git branch -- see local branches ```
- ```git branch -r -- see remote branches```
- ```git branch -a -- see local and remote```
- ```git rev-parse --abbrev-ref HEAD -- to display only the name of the current branch you're on ```

- Delete remote branch

``` git push origin --delete [branch] ```

# Change remote path

```
git remote -v 

git remote set-url origin git@github.com:Career-Karma-Tutorials/git-submodule.git (new)
```
