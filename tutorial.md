[On the invention of git](https://www.git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git)!
For more details (on everything in this session), 
see the [Git Pro](https://www.git-scm.com/book/en/v2) book.

Our starting point is 

![XKCD 1597](https://imgs.xkcd.com/comics/git_2x.png)

or "git without branches": only the bare-bones functionality of git, but still better than 
not doing versioning at all!

### Git without branches

Cloning a repository

```
git clone https://github.com/fellowship-of-clean-code/git-versioning
cd git-versioning
```

Showing the state of the repo

```
git log
git log --oneline
git log --oneline --graph
```

Use [a better git log](https://coderwall.com/p/euwpig/a-better-git-log):

```
git config --global alias.lg "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(auto)%d%C(reset)'"
git lg
git lg -p
```

Making a commit

```
git add <filename(s)>
git commit -m "<commit message>"
```

Short diversion: [meaningful commit messages](https://www.conventionalcommits.org/en/v1.0.0/).
Format commit messages like 

```
git commit -m "<type>: <did something>" -m "<longer description (optional)>"
```

Short diversion: `git add -i`, a way to interactively select which changes you want 
to stage to be committed.

The dangers of `git add .`

Also, `git status` and `git reset`.

At this point, we can already make use of the version history represented
by the sequence of commits!

Two examples are as follows: we can see exactly was changed between one commit and
another with 

```
git diff <sha-initial> <sha-final>
```

or, if we want to only see what was changed in a specific commit, simply

```
git diff <sha>^!
```

This is all local: in order to sync to the remote repository, 
we use, in order (and with simplified syntax): 

- clone repo
- repeat:
    - make changes
    - `git add`
    - `git commit`
- `git pull`
- `git push origin main`

This is the most basic git workflow, but it has a big issue:
it does not allow us to "safely break code". 
If we are hacking away, modifying functionality and potentially breaking stuff, 
when suddently we need to have a working copy of the code, that can be a problem! 

This (among others) is the problem branches solve.

### Git with branches

Making a branch & the branching picture:

```
git branch <branch-name>
git checkout <branch-name>
```

or simply 

```
git checkout -b <branch-name>
```

How to move around between branches:

```
git checkout <branch-name>
```

When the work in a branch is complete, it can be integrated into the main branch: 
this is accomplished by 

```
(git checkout main)
git merge <feature-branch-name>
```

If the branches modified different sections of the files or different files altogether,
there is no problem.
If, however, the same section of the same file is modified in both branches 
there is ambiguity: which should we pick? 

After git points this out (a "merge conflict") there are two options: 

- one may abort the merge, realizing that the branch was not ready:
    `git merge --abort`
- one may resolve the conflicts in favor of one of the sides.

This can be done manually, by looking at the files, or by using a mergetool.

Git push and git pull:
the state of your local branch will not, in general, be the same as the one of the
one on the server. You may have made new changes (i.e. commits), and 
somebody else might also have made changes.

In order to ensure that no conflicts are created, git will force you to start by 
running `git pull`: download new commits from the server, and try to merge them 
with the local changes.

Once your branch is in sync with the "origin" branch on the server, you can 

```
git push origin master
```

or, after the first time, simply 

```
git push
```

(optional) [Making a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests), issues.

(optional) .gitignore: don't commit your trash!

(optional) .gitkeep: force-commit folders

### Exercises

[Create a new repository](https://github.com/new) on your github profile!
I'd advise making it private and with a meaningless name, since it's just for this exercise,
but you do you.

After creating, follow the instructions to initialize an empty repository of the same name 
locally. 

You now have a repo! With the commands discussed earlier, and looking up anything which is missing,
try to do any number of the following:

- commit to the `main` branch, and push to the server: you can now view your commit on github as well!
- open a `new-feature` branch, commit to it; 
    move back to the `main` branch and commit something unrelated, 
    now merge the `new-feature` branch into the `main` branch!
- bring your repo back into the past with `git checkout <commit-sha>`, and come back to the present!
- now, repeat the same procedure but, instead of locally merging the `new-feature` branch,
    open a pull request on your own repo for the new feature.
- (extra) open an issue on your own repo, and reference it in your pull request!
- set up your commit message convention, perhaps based on what was discussed above, and try sticking to it.
- commit a long file into the repo, 
    make two changes to it, and try committing only one of them (without touching the file anymore);
    the commands `git add --patch` or `git add -i` may help.
- (extra) same setup as before: `main` and `new-feature` branches with diverging histories;
    now, instead of merging the changes from the `new-feature` branch, try [rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
    the branch with the changes from the main branch.

Advanced exercise: **contributing to an open source project**!

- Find a public repository you like / often use;
- check out their [contribution guidelines](https://docs.astropy.org/en/latest/development/workflow/development_workflow.html),
    (example from astropy) and follow them;
    - for example, astropy maintainers ask you to rebase instead of merging!
- look through their issues marked as ["good first issues"](https://github.com/astropy/astropy/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) (example from astropy)
- find one which it looks like you can fix
- alternatively, if there's something which bothers you, open an issue yourself!
- comment on the issue, stating your intention to work on it, and asking for clarifications if needed 
- fork the repo, make a branch for your fix
- commit your fix proposal to the branch
- open a pull request, stating which issue you are proposing a fix for 
    (it's ok if the fix is not fully functional yet)
- collaborate with the repo's owners to improve your proposal
- get your pull request approved! 
