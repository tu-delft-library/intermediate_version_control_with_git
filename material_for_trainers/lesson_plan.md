
## 9:00 - Land - 5'- CATA
☕ Coffee/tea 🫖

## 9:05 - Housekeeping - 15" CATA
- ✅ Roll call + 🤝 Code of Conduct
- 🙋 Getting help (🆘 red  ✅ green stickers)

## 9:20 - Icebreaker - 5'- CATA
🎥 Icebreaker instructions on slides
> *START AUTOPUSH* 


## 9:40 - Generate local repo from assignment - 10'- CATA  

Below is the minimal set of instructions to generate the repository for today.


```bash
cd Desktop
mkdir weather-notes
cd weather-notes/
git init
touch README.md
touch LICENSE
git add README.md LICENSE 
git commit -m "Initial commit: add README and LICENSE"
echo "The sun came out" > notes.txt 
git add notes.txt 
git commit -m "Add first line"
echo "The air was fresh" >> notes.txt  
git add notes.txt 
git commit -m "Add second line"
echo "A cloudy afternoon" >> notes.txt 
git add notes.txt 
git commit -m "Add third line"
touch debug.log
mkdir data
touch data/raw_dump.csv
touch data/temperatures.csv
echo "*.log" > .gitignore
echo "data/*" >> .gitignore
git add .gitignore
git commit -m "Ignore all log and data files"
```
This should generate a local repository with a commit history. But we still need to add the remote repository:

- Create a new empty repository on GitHub (no README/license — you already have those).
- Connect your local repo to it and push all your commits. Replace <your-user-name> with your GitHub user name
```bash
git remote add origin git@github.com:<your-user-name>/weather-notes.git
git push origin main
```
- Refresh the GitHub page and confirm every file and all commits made it across.
- Confirm locally that we have all the changes 

```bash
git log --oneline
```
> 🆘 red  ✅ green stickers

Ask if everyone has the `weather-notes` repository with a similar history:
- 1 initial commit
- 3 commits for editing `notes.txt`
- 1 `.gitignore` commit

## 9:25 - Introduction to branches - 10'- CATA 
🎥 Use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20version%20control%20with%20Git.pptx?d=w33d15b9f24e94794aaa7624d5b908dd3&csf=1&web=1&e=rgIzM0)

## 9:50 - New commands for branching - 10'- CATA 

```bash
git branch              # check branches (explain the * pointing to main) 
git branch b1           # add argument to create new branch
git branch              # verify branch was created (output: b1, *main)
```
`b1` is a name. It can be any name. Recommendations: use lowercase and underscores/dashes
```bash
git status              # on branch main / nothing to commit
cat notes.txt           # three lines
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
Output should look like: 
```bash
hash (HEAD -> main, origin/main, b2, b1) Ignore all log and data files
```

## 10:05 - 1 💪 Get familiar with branches - 10'- CATA 
See `exercises.md`. There is an optional challenge under each numbered exercise.

Solution:
```bash
git branch pa
git branch pe
git switch -c pi
git branch -m pa po
git switch po
git log --oneline
git switch pe
git log --oneline
git switch main         # stand on main to delete other branches
git branch -d pe pi po
git log --oneline
```

## 10:15 - Develop on different branches - 10'- CATA 


```bash
git status
echo "A dramatic sunset" >> notes.txt              # add more lines
git diff
git add notes.txt 
git commit -m "Add fourth line"    # commit changes on main
git status
git switch b1
git log --oneline
echo "A dramatic sunset" >> notes.txt 
git diff
git add notes.txt
git commit -m "Add fourth line on branch b1" # commit changes on b1
git status
git switch main
git log --oneline --all                    # show all branches
git log --oneline --all --graph            # show graph with all branches
```
Notice the HEAD pointing to the main branch. The commits are different even if the changes are similar.

## 10:25 - 2 💪 Commit in a secondary branch - 5'- CATA
See `exercises.md`. There is an optional challenge under each numbered exercise. 

Solution:
```bash
git switch b2
cat notes.txt
echo "A dramatic sunset" >> notes.txt 
echo "The moon was bright" >> notes.txt
git diff
git add notes.txt
git commit -m "Add two more lines on b2"
git status
```

## 10:30 - Break -10' 


## 10:40 - Explore differences across branches - 15'- HALFORD 

Let's keep adding to our history:
```bash
git switch main
cat notes.txt           # 4 lines on main
echo "A dramatic sunset (duplicate)" >> notes.txt      # add mistake on main
git diff
git add notes.txt 
git commit -m "Add fifth line on main (with mistake)"
cat notes.txt               # 5 lines (5th one is a duplicate)
git switch b1
cat notes.txt               # only 4 lines
echo "The moon was bright" >> notes.txt 
git diff                    # 5 lines (5th line is new)
git add notes.txt 
git commit -m "Add fifth line on b1"
git log --oneline --all --graph                 # so many little changes
```
Maybe we are confused about what was done on which branch. Let's see how to investigate the changes.

```bash
git switch main                     
git diff HEAD HEAD~1      # REMEMBER: changes between latest and one before latest commit
git diff main b1 # changes between  main and b1
git diff main b1~1 # changes between main and parent of the latest commit on branch B1
git diff main~1 b1~1 # changes between parent of the latest commit on the main branch and the parent of the latest commit on branch B1
# No difference on this one. Both files have 4 lines
```

## 10:55 - 3 💪  Explore differences across branches - 10'- HALFORD

See `exercises.md`. There is an optional challenge under each numbered exercise.

Solution:
```bash
git switch b2
git diff HEAD
git diff HEAD~1
git diff HEAD~2
git diff HEAD~3         
git diff HEAD~4         
git diff HEAD~5         
git diff HEAD~6         # fails -> current history only has 6 commits
git diff b2 b1
git diff main b2~1
```

## 11:05 - Merging branches and conflict resolution - 15'- HALFORD 

Let's develop further on branch `b1`:
```bash
git switch b1                                             # on branch b1
echo "It rained at night" >> notes.txt            # routine
echo "Give me my thick blanket" >> notes.txt      # routine
git diff                                                  # routine
git add notes.txt                                         # routine
git commit -m "Add sixth and seventh lines on b1"   # routine
git log --oneline --all --graph                           # routine
```
We are done with developing on `b1` and want to merge to `main`. 
For this we first *stand* on main branch (with `switch`) and then apply the merge.
```bash
git switch main                     # on branch main
git diff main b1                    # verify
git merge -m "Merge b1" notes.txt   # wrong syntax - merge expects branch not file
git merge -m "Merge b1" b1          # right syntax: fails because of conflict
git status                          # see which files are in conflict
git diff notes.txt                  # verify differences
```
To resolve the merge conflict:
1. edit manually the conflicting sections: keep current branch (aka `ours`), incoming branch (aka `theirs`), combine or make entirely different changes.
1. remove the conflict markers

> **Reading the markers:**
> - `<<<<<<< HEAD` → your current branch's version
> - `=======` → dividing line
> - `>>>>>>>` → the incoming branch's version

```bash
nano notes.txt           # edit the file within the conflict markers
                         # manually combine both sets
                         # demo ^k to delete whole line
```
`notes.txt` should look like this:
```bash
The sun came out
The air was fresh
A cloudy afternoon
A dramatic sunset
The moon was bright
It rained at night
Give me my thick blanket 
```
>
In this example, the conflict has been resolved by keeping the changes from branch `b1`.

```bash
git status
git diff                          # diff shows nothing! merge state - similar to staging
```
During a merge, conflicting files go to a different state while the conflict is resolved and a commit is done.
The easiest way to see the state of the file is to see show the content using `cat`.

```bash
cat notes.txt
git add notes.txt                                         # mark resolution
git status                                                # verify
git commit -m "Merge changes from b1 into main"          # conclude merge right syntax
git status                                                # verify
git log --oneline --all --graph                           # see merge visually
```

> **ADVANCED** If you really want to see the differences during a merge use: `git diff :1:notes.txt :2:notes.txt`

## 11:20 - 4 💪 A first type for merge - 10'- HALFORD 
See `exercises.md`. There is an optional challenge under each numbered exercise.



Solution:
```bash
git diff main b2                # show differences all files - same in this case
git switch main                 # stand on main
git merge b2                    # merge b2 into main
nano notes.txt                  # fix conflicts
git add notes.txt
git commit -m "Merge b2 into main"
git log --oneline --all --graph # verify
```

> **Before break:** Turn to a neighbour and compare the output of `git log --oneline --all --graph`. Does it look the same? Different commit hashes? Same shape?

## 11:30 - Break  -10'

> CONTINUE HERE!

## 11:40 -  💻 PRACTICAL - Understanding merge conflicts - 40'- CATA
see `PRACTICAL_merge_conflicts.md`

Do not solve the PRACTICAL live. Just ask questions, share experiences or highlight concepts that you noticed were still a bit confusing.

## 12:30 - 	Lunch - 60'		

## 13:30 - 5 💪 Interactive Git - 15'- HALFORD 
See `exercises.md`. There is an optional challenge under each numbered exercise.

## 13:50 - Remote operations revisited - 10'- HALFORD 

🎥 Use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20version%20control%20with%20Git.pptx?d=w33d15b9f24e94794aaa7624d5b908dd3&csf=1&web=1&e=rgIzM0)

Explain remote operations:
- **Fetching**: downloading new data (commits, branches, or tags) from a remote repository into your local repository — without modifying your working files.
- **Pushing**: uploading your local commits to the remote repository.
- **Pulling**: `git pull` is `git fetch` + `git merge` in one step.

## 14:10 - Solve a conflict when pushing - 15'- HALFORD

Make a small edit directly on GitHub (via the web editor):
1. Open `notes.txt` on GitHub
2. Add a new line at the bottom: `or a hot water bottle - GitHub`
3. Commit with message `Add eighth line via GitHub`

Now, back in the terminal, make a **different** local change without pulling:
```bash
echo "or a hot water bottle - local" >> notes.txt
git add notes.txt
git commit -m "Add eighth line locally'
git push origin main        # fails! rejected - remote contains work you do not have
```

Explain **divergent branches**: both the local and remote `main` have moved forward independently. Git can try to reconcile the branches. The two most common options are: `merge` and `rebase`.

Let's ask Git to use `merge` by default
```bash
git config pull.rebase false      # merge
```

Resolve it:
```bash
git pull origin main        # fetch + attempt merge → conflict in notes.txt
git status                  # notes.txt listed as "both modified"
nano notes.txt              # resolve conflict markers — keep one or combine both lines
git add notes.txt
git commit -m "Merge remote and local eighth line'
git push origin main        # now succeeds
```

Visit GitHub and confirm the resolved file is there.

> **Key message:** the conflict resolution steps are identical whether the divergence comes from a colleague or from your own edit on GitHub. **Pull before you push.**

## 14:25 - 6 💪 Undo a Bad Merge - 10'- HALFORD
See `exercises.md`. There is an optional challenge under each numbered exercise.

Solution:
```bash
git switch main
git branch bad-merge
git switch bad-merge
echo "to stay warm at night - branch version" >> notes.txt
git add notes.txt
git commit -m "Add ninth line on bad-merge"
git switch main
echo "to stay warm at night - main version" >> notes.txt
git add notes.txt
git commit -m "Add ninth line on main"
git merge bad-merge         # conflict! both branches changed the last line
git status                  # notes.txt listed as "both modified"
cat notes.txt               # conflict markers are visible
git merge --abort
git status                  # clean — back to where you were before the merge
cat notes.txt               # conflict markers are gone, file is as it was on main
git log --oneline --graph   # no merge commit was created
git branch -D bad-merge     # force-delete (it was never cleanly merged)
git log --oneline --graph   # back to a clean main
```

`git merge --abort` is only available **while a merge is in progress** 

## 14:35 - Break -10" 

## 14:45 - 💻 PRACTICAL - Conflicts with Remote Repositories - 40'- CATA 
see `PRACTICAL_remote_conflicts.md`

Do not solve the PRACTICAL live. Just ask questions, share experiences or highlight concepts that you noticed were still a bit confusing.


## 15:50 - Demo git operations in VSCode - 10'- CATA
- Some people prefer to use a GUI to work with Git.
- Let's explore that using VSCode
[TODO] add more explanation about VS code --> what it is and why it has a terminal etc


### Git by default
- Open VSCode
- Open folder -> recipes folder
- Go to git tab (left)
- Explain GUI:
    - log -> hover for details
    - click on +- icon on the right to show changes
    - right click for more options
### Commit changes
- Open `guacamole.md` from explorer
- Make a change (e.g. smash avocado, add salt, pepper and lime)
- Save `guacamole.md` (CTRL + S)
- Notice badge on git icon
- Click on `guacamole.md` to see the changes on the right
    - red deleted
    - green added
- Click on plus to stage
- Write message and click on commit
- Notice the update on the log
- Push by clicking "Publish Branch"
- Confirm in GitHub


## 15:45 - Summarize key points - 10'- HALFORD 
- **Branches**: create isolated lines of development with `git branch` and `git switch`.
- **Merging**: bring changes together with `git merge`.
Git creates a merge commit
- **Conflicts**: happen when the same lines were changed in both branches. Always: edit → remove markers → `git add` → `git commit`
- **Remote workflows**: `clone`, `push`, `pull`. Pull before you push. Conflicts can happen on remotes too, and are resolved the same way
- **Escape hatches**: if a conflict surprises you and you need time to think `git merge --abort` is a safe exit

## 15:55 - Give feedback about the course  5" 
Go to the link in `README.md`


