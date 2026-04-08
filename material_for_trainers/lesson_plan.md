
## 9:00 - Land - 10'
☕ Coffee/tea 🫖

## 9:10 - Housekeeping - 10'
- ✅ Roll call + 🤝 Code of Conduct
- 🖥 Did everyone:
    - install git
    - create a GitHub account
    - set up SSH key
- 🙋 Getting help (🆘 red  ✅ green stickers)

## 9:20 - Icebreaker - 5'
🎥 Icebreaker instructions on slides

## 9:25 - Introduction to branches - 15' 
🎥 Use slides

## 9:40 - Make local repo with history - 10' 
Use this opportunity to **recap git commands**

Let's configure our `git` default editor:
```bash
git config --global core.editor "nano -w"
```
```bash
cd Desktop/
mkdir sandbox
cd sandbox/
git init
git status
echo 'first line'                       # echo prints text to terminal screen
echo 'first line' > lines.txt           # > redirects string into a file, overwrites content
echo 'second line' >> lines.txt         # >> similar to > but it appends instead of overwrite
git add lines.txt 
git commit -m 'Add first two lines' lines.txt 
git log --oneline
echo 'third line' >> lines.txt 
git add lines.txt 
git commit -m 'Add third line' lines.txt
git log --oneline
```
## 9:50 - New commands for branching - 15' 

```bash
git branch              # check branches (explain the * pointing to main) 
git branch b1           # add argument to create new branch
git branch              # verify branch was created (output: b1, *main)
```
`b1` is a name. It can be any name. Recommendations: use lowercase and underscores/dashes
```bash
git status              # on branch main / nothing to commit
cat lines.txt           # three lines
git log --oneline       # explain (HEAD -> main, b1)
git branch -m b1 b2     # rename branch (-m for move)
git log --oneline       # branch b1 was renamed to b2 (HEAD -> main, b2)
git branch -d b2        # delete branch
git log --oneline       # verify main is the only branch
git branch -d main      # fails - can't delete current branch
git branch b1           # create again branch b1
git log --oneline       # verify
git switch              # fails - no branch name
git switch b1          # stand on branch b1
git log --oneline       # verify
git branch b1           # branch alreay exists 
git switch -c b2        # create and switch to b2
git log --oneline       # notice HEAD -> b2
git switch main         # switch to main
git log --oneline       # notice HEAD -> main
```

## 10:05 - 💪 Get familiar with branches - 10' 
See `exercises.md`

Solution:
```bash
git branch pa
git branch pe
git branch pi
git branch -m pa po
git switch po
git log --oneline
git switch pe
git log --oneline
git switch main         # stand on main to delete other branches
git branch -d pe pi po
git log --oneline
```

## 10:15 - Develop on different branches - 10' 


```bash
git status
echo 'forth line' >> lines.txt              # add more lines
git diff
git add lines.txt 
git commit -m 'Add forth line' lines.txt    # commit changes on main
git status
git switch b1
git log --oneline
echo 'forth line' >> lines.txt 
git diff
git add lines.txt
git commit -m 'Add forth line on branch b1' lines.txt # commit changes on b1
git status
git switch main
git log --oneline --all                    # show all branches
git log --oneline --all --graph            # show graph with all branches
```
Notice the HEAD pointing to the main branch. The commits are different even if the changes are similar.

## 10:25 - 💪 Commit in a secondary branch - 5'
see `exercises.md` 

Solution:
```bash
git switch b2
cat lines.txt
echo 'forth line' >> lines.txt 
echo 'fifth line' >> lines.txt
git diff
git add lines.txt
git commit -m "Add two more lines on b2"
git status
```

## 10:30 - Break - 10' 

## 10:40 - Explore differences across branches - 15'

Let's keep adding to our history:
```bash
git switch main
cat lines.txt
echo 'forth line (duplicate)' >> lines.txt      # add mistake on main
git diff
git add lines.txt 
git commit -m 'Add fifth line on main (with mistake)' lines.txt 
cat lines.txt 
git switch b1
cat lines.txt
echo 'fifth line' >> lines.txt 
git diff
git add lines.txt 
git commit -m 'Add fifth line on b1' lines.txt 
git log --oneline --all --graph                 # so many little changes
```
Maybe we are confused about what was done on which branch. Let's see how to investigate the changes.

```bash
git switch main                     
git diff HEAD HEAD~1 lines.txt      # REMEMBER: changes between latest and one before latest commit
git diff main b1 lines.txt # changes between  main and b1
git diff main b1~1 lines.txt # changes between main and parent of the latest commit on branch B1
git diff main~1 b1~1 lines.txt # changes between parent of the latest commit on the main branch and the parent of the latest commit on branch B1
# No difference on this one. Both files have 4 lines
```
Let's correct the mistake on `main` branch

```bash
git status              # verify
nano lines.txt          # change "forth line (duplicate)" with "fifth line"
cat lines.txt           # verify
git status              # verify
git add lines.txt       # routine
git commit -m 'Correct fifth line on main' lines.txt # routine 
git log --oneline --graph
```

## 10:55 - 💪  Explore differences across branches - 10'
see `exercises.md`

Solution:
```bash
git switch b2
git diff HEAD
git diff HEAD~1
git diff HEAD~2
git diff HEAD~3         # fails
git diff b2 b1 lines.txt
git diff main b2~1 lines.txt
```

## 11:05 - Merging branches and conflict resolution - 15'

Let's develop further on branch `b1`:
```bash
git switch b1                                             # on branch b1
echo 'sixth line' >> lines.txt                          # routine
echo 'seventh line' >> lines.txt                          # routine
git diff                                                  # routine
git add lines.txt                                         # routine
git commit -m 'Add 6th and 7th lines on b1' lines.txt   # routine
git log --oneline --all --graph                           # routine
```
We are done with developing on `b1` and want to merge to `main`. 
For this we first *stand* on main branch (with `switch`) and then apply the merge.
```bash
git switch main                     # on branch main
git diff main b1 lines.txt          # verify
git merge -m 'Merge b1' lines.txt   # wrong syntax - merge expects branch not file
git merge -m 'Merge b1' b1          # right syntax: fails because of conflict
git status                          # see which files are in conflict
git diff lines.txt                  # verify differences
```
To resolve the merge conflict:
1. edit manually the conflicting sections: keep current branch (aka `ours`), incoming branch (aka `theirs`), combine or make entirely different changes.
1. remove the conflict markers

> **Reading the markers:**
> - `<<<<<<< HEAD` → your current branch's version
> - `=======` → dividing line
> - `>>>>>>>` → the incoming branch's version

```bash
nano lines.txt           # edit the file within the conflict markers
                         # manually combine both sets
                         # demo ^k to delete whole line
```
`lines.txt` should look like this:
```bash
first line
second line
third line
forth line
fifth line
sixth line
seventh line
```
>
In this example, the conflict has been resolved by mixing both sets of changes. Parts of the changes from both branches are now present in the file. If you are done you can use `git add` to stage the resolved changes:

```bash
git status
git diff                                                  # diff shows nothing!
```
During a merge, conflicting files go to a different state while the conflict is resolved and a commit is done.
The easiest way to see the state of the file is to see show the content using `cat`.
```bash
cat lines.txt
git add lines.txt                                         # mark resolution
git status                                                # verify
git commit -m 'Merge changes from b1 into main'          # conclude merge right syntax
git status                                                # verify
git log --oneline --all --graph                           # see merge visually
git log --oneline --all --graph --parents                 # see merge with hashes 
```

> **ADVANCED** If you really want to see the differences during a merge use: `git diff :1:lines.txt :2:lines.txt`

## 11:20 💪 A first type for merge - 10'
see `exercises.md`

Solution:
```bash
git diff main b2 lines.tx       # show differences for lines.txt
git diff main b2                # show differences all files - same in this case
git switch main                 # stand on main
git merge b2                    # merge b2 into main
nano lines.txt                  # fix conflicts
git add lines.txt
git commit -m 'Merge b2 into main'
git log --oneline --all --graph # verify
```

> **Before break:** Turn to a neighbour and compare the output of `git log --oneline --all --graph`. Does it look the same? Different commit hashes? Same shape?

## 11:30 - Break  - 10' 

## 11:40 -  💻 LAB - Understanding merge conflicts - 40'
see `LAB_merge_conflicts.md`

## 12:20 -  Review LAB with group - 10'
Do not solve the LAB live. Just ask questions, share experiences or highlight concepts that you noticed were still a bit confusing.

## 12:30 - 	Lunch - 60'		

## 13:30 💪 Interactive Git - 15'
- Go to this link https://learngitbranching.js.org/
- Complete **Introduction episodes 1, 2, 3**
- [Optional] Continue on the more advanced exercises

## 13:45 - Create a remote repository on GitHub - 5'

Our `sandbox` has grown into something we want to keep. Time turn it into a remote repository.

Because our local history is a little messy, we prefer to have a fresh clean repo and copy the current state of the files there.

1. Go to [github.com](https://github.com) and sign in
2. Click the **+** icon in the top-right corner and choose **New repository**
3. Name it `logbook`
4. Leave it **Public**
5. Leave all clone_colleague options as they are. Do **not** add a `README` or `.gitignore`
6. Click **Create repository**

GitHub will show you your new empty repository and its URL. 
Click on the `SSH` tab. It will look like:
```
git@github.com:YOUR-USERNAME/logbook.git
```
Copy this URL

## 13:50 - Cloning and pushing - 10'

Clone the repository locally:
```bash
cd ~/Desktop
git clone git@github.com:YOUR-USERNAME/logbook.git  # empty repo warning is ok!
cd logbook
```

```bash
git status                                        # nothing to commit
git branch                                        # observe that is empty
git log                                           # observe - fatal error -> no branch
```

```bash
git remote -v               # shows short name (origin) and full URL
```

Explain remote operations:
- **Fetching**: downloading new data (commits, branches, or tags) from a remote repository into your local repository — without modifying your working files.
- **Pushing**: uploading your local commits to the remote repository.
- **Pulling**: `git pull` is `git fetch` + `git merge` in one step.

Copy the local `sandbox` file into this repo and push:
```bash
cp ~/Desktop/sandbox/lines.txt .
git add lines.txt
git commit -m 'Add lines.txt from local work'
git push origin main        # push to remote
```
Note the message from git showing the commits were pushed to Github.

Visit GitHub and refresh — students should see `lines.txt` appear online.

## 14:00 - Pulling from a remote repository - 10'
Let's pretend someone else edited the file.
We'll simulate this by using GitHub web editor:
1. Open `lines.txt` on GitHub
1. Click on the pencil icon on the upper right corner to edit
1. Add a new line at the bottom: `eighth line`
1. Commit with message `Add eighth line via GitHub`

Now pull it locally:
```bash
git log --oneline           # notice the remote change is not here yet
git pull origin main        # fetch + merge in one step
git log --oneline           # now the commit from GitHub is visible
cat lines.txt               # eighth line is there
```

> **Key message:** always pull before you start working to avoid unnecessary conflicts.

## 14:10 - Solve a conflict when pushing - 15'

Make a small edit directly on GitHub (via the web editor):
1. Open `lines.txt` on GitHub
2. Add a new line at the bottom: `ninth line - GitHub`
3. Commit with message `Add ninth line via GitHub`

Now, back in the terminal, make a **different** local change without pulling:
```bash
echo 'ninth line - local' >> lines.txt
git add lines.txt
git commit -m 'Add ninth line locally'
git push origin main        # fails! rejected - remote contains work you do not have
```

Explain **divergent branches**: both the local and remote `main` have moved forward independently. Git can try to reconcile the branches. The two most common options are: `merge` and `rebase`.

Let's ask Git to use `merge` by default
```bash
git config pull.rebase false      # merge
```

Resolve it:
```bash
git pull origin main        # fetch + attempt merge → conflict in lines.txt
git status                  # lines.txt listed as "both modified"
nano lines.txt              # resolve conflict markers — keep one or combine both lines
git add lines.txt
git commit -m 'Merge remote and local ninth line'
git push origin main        # now succeeds
```

Visit GitHub and confirm the resolved file is there.

> **Key message:** the conflict resolution steps are identical whether the divergence comes from a colleague or from your own edit on GitHub. **Pull before you push.**

## 14:25 - 💪 Undo a Bad Merge - 10'
see `exercises.md`

Solution:
```bash
git switch main
git branch bad-merge
git switch bad-merge
echo 'tenth line - branch version' >> lines.txt
git add lines.txt
git commit -m 'Add tenth line on bad-merge'
git switch main
echo 'tenth line - main version' >> lines.txt
git add lines.txt
git commit -m 'Add tenth line on main'
git merge bad-merge         # conflict! both branches changed the last line
git status                  # lines.txt listed as "both modified"
cat lines.txt               # conflict markers are visible
git merge --abort
git status                  # clean — back to where you were before the merge
cat lines.txt               # conflict markers are gone, file is as it was on main
git log --oneline --graph   # no merge commit was created
git branch -D bad-merge     # force-delete (it was never cleanly merged)
git log --oneline --graph   # back to a clean main
```

`git merge --abort` is only available **while a merge is in progress** 

## 14:35 - Break - 10' 

## 14:45 - 💻 LAB - Conflicts with Remote Repositories - 40' 
see `LAB_remote_conflicts_github.md`

## 15:25 - Review LAB with the group - 10'
Do not solve the LAB live. Just ask questions, share experiences or highlight concepts that you noticed were still a bit confusing.

## 15:35 - Break - 10'
## 15:45 - Summarize key points - 10'
- **Branches**: create isolated lines of development with `git branch` and `git switch`.
- **Merging**: bring changes together with `git merge`.
Git creates a merge commit
- **Conflicts**: happen when the same lines were changed in both branches. Always: edit → remove markers → `git add` → `git commit`
- **Remote workflows**: `clone`, `push`, `pull`. Pull before you push. Conflicts can happen on remotes too, and are resolved the same way
- **Escape hatches**: if a conflict surprises you and you need time to think `git merge --abort` is a safe exit


## 15:55 - Give feedback about the course - 5' 
Go to the link in `README.md`
