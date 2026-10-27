> Adapted from *Version Control and Collaborative Development for Research Software —GitCoDev*, TUDelft, licensed [CC-BY 4.0](https://4turesearchdata-carpentries.github.io/GitCoDev/licensing.html).

## 9:00 🚀 Prepare for take off - 20'

### Room setup

- Make sure to plug your computer to electricity. Otherwise the display will feed electricity to the laptop and potentially turn itself off.
- Test the microphone
- Write the edu.nl link pointing to `material_for_participants/links.md` document on the whiteboard
  - Use dark marker on the board (not red)
- Place the list of the participants on a table at the entrance so that they can check their name (roll call)
- [Optional] bring jugs with water and snacks (if you brought any)

### Laptop setup

#### start autopush

Make sure to set up the bash history forwarding before workshop day as explained in `materials_for_trainers/pre_workshop_checklist.md`

Open another Terminal window. Let's call this the `hidden terminal` i.e. NOT to be shown during the workshop.

On the `hidden terminal` forward only the last line of the history to `command.log`

```
tail -n 0 -f ~/.bash_history | tee -a <local-repo-directory>/material_for_participants/command.log
```

Open another tab in the `hidden terminal`, `cd` to `<local-repo-directory>` and run autopush every 2-5 seconds

```
gitautopush --sleep 10 .
```

#### Open slides

Slides work better if you open them in your device: [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20version%20control%20with%20Git.pptx?d=w33d15b9f24e94794aaa7624d5b908dd3&csf=1&web=1&e=rgIzM0)

## 9:30 - Participants land - 5' - TRAINER ONE

☕ Remind them about coffee at the entrance of library

## 9:35 - Housekeeping - 5' - TRAINER ONE

- ✅ Roll call + 🤝 Code of Conduct
- 🙋 Getting help (🆘 red ✅ green stickers)

## 9:40 - Icebreaker - 5'- TRAINER ONE

🎥 Icebreaker instructions on slides

> *START AUTOPUSH*

## 9:45 - Check your starting point - 10' - TRAINER ONE

> Because we are doing trainer split, both trainers work on the same remote repository. Before the workshop: 
> - ensure your `free_fall` repository is at the same state after `Git fundamentals` day one` + home assignment. Check [the solution from the home assignment](https://github.com/tu-delft-library/introduction_to_version_control_with_git/blob/main/material_for_participants/PRACTICAL_putting_it_all_together.md#%EF%B8%8Fsolution-to-start-this-practical%EF%B8%8F)
> - The owner of the repo should add the other trainer as collaborator.

Today we continue in the repository we started last time. Everyone moves into it and checks the state:

```
cd ~/Desktop/free_fall
git status                  # clean working tree
git remote -v               # origin points to my own GitHub repository
git log --oneline           
ls                          # LICENSE	README.md	distance.py	time.py	velocity.py data debug.log
cat velocity.py
```

`velocity.py` should look like this:

```
# Final velocity of an object after free falling during a given time
gravity = 9.81
time = 2.0
velocity = gravity * time
print("After", time, "s a free falling object reaches", velocity, "m/s")
```
Ask if everyone has a similar history (the exact number of commits will differ):

- the commits from day one
- 3 commits for creating and editing `velocity.py`
- 1 `.gitignore` commit on top

And that `git status` says the branch is up to date with `origin/main`.

Ask participants who did not finish the home assignment, or whose repo got messy, to rebuild the starting point. Follow the solution [from the home assignment](https://github.com/tu-delft-library/introduction_to_version_control_with_git/blob/main/material_for_participants/PRACTICAL_putting_it_all_together.md#%EF%B8%8Fsolution-to-start-this-practical%EF%B8%8F)

> 🆘 red ✅ green stickers

## 9:55 - Introduction to branches - 10' - TRAINER ONE

🎥 Use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20version%20control%20with%20Git.pptx?d=w33d15b9f24e94794aaa7624d5b908dd3&csf=1&web=1&e=rgIzM0)

## 10:05 - New commands for branching - 10' - TRAINER ONE

```
git branch              # check branches (explain the * pointing to main)
git branch b1           # add argument to create new branch
git branch              # verify branch was created (output: b1, *main)
```

`b1` is a name. It can be any name. Recommendations: use lowercase and underscores/dashes

```
git status              # on branch main / nothing to commit
python velocity.py      # After 2.0 s ... 19.62 m/s
git log --oneline       # explain (HEAD -> main, origin/main, b1)
git branch -m b1 b2     # rename branch (-m for move)
git log --oneline       # branch b1 was renamed to b2 (HEAD -> main, origin/main, b2)
git branch -d b2        # delete branch
git log --oneline       # verify main is the only branch (local and remote)
git branch -d main      # fails - can't delete current branch
git branch b1           # create again branch b1
git log --oneline       # verify
git switch              # fails - no branch name
git switch b1           # stand on branch b1
git log --oneline       # verify
git branch b1           # branch already exists
git switch -c b2        # create and switch to b2
git log --oneline       # notice HEAD -> b2
git switch main         # switch to main
git log --oneline       # notice HEAD -> main
```

Output should look similar to (`origin/main` decoration may appear in a different order):

```
hash (HEAD -> main, origin/main, b2, b1) Ignore log and data files
```

## 10:15 - 1 💪 Get familiar with branches - 10' - TRAINER ONE

See `exercises.md`. There is an optional challenge.

Solution:

```
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

## 10:25 - Develop on different branches - 10' - TRAINER ONE

```
git status
python velocity.py          # After 2.0 s ... 19.62 m/s
nano velocity.py            
```
Convert the velocity to km/h
```python
velocity_kmh = velocity * 3.6
``` 
```
git diff
git add velocity.py
git commit -m "Convert velocity to km/h"    # commit changes on main
git status
git switch b1
git log --oneline
nano velocity.py          # same change on b1
```
```python
velocity_kmh = velocity * 3.6
```
```
git diff
git add velocity.py
git commit -m "Convert velocity to km/h on b1"   # commit changes on b1
git status                                  # nothing to commit
cat velocity.py                             # see content in b1
git switch main
cat velocity.py                             # see same contents of file in main and b1
git log --oneline --all --graph             # NEW COMMAND: see all branches + graph
```

Notice the HEAD pointing to the main branch. The **commits are different** even if the changes are similar. The commits have the metadata about branch.

This command is long and we will type it many times. Let's make a shortcut (an *alias*). `-n 10` shows only that last 10 commits.

```
git log --oneline --all --graph -n 10      # limit to last 10 commits
git config --global alias.lg "log --oneline --all --graph -n 10"
git lg                      # same output, much shorter to type
```

From now on we use `git lg`.

## 10:35 - 2 💪 Commit in a secondary branch - 5' - TRAINER ONE

See `exercises.md`.

Solution:

```
git switch b2
nano velocity.py            
```
Add conversion formula
```python
velocity_kmh = velocity * 3.6
```
```
git diff
git add velocity.py
git commit -m "Convert velocity to km/h on b2"
git status
git lg
```

## 10:40 - Break - 15'

> **Trainer change** push and pull git@github.com:catactg/free\_fall.git

## 10:55 - Explore differences across branches - 10' - TRAINER TWO

> *START AUTOPUSH*

Let's keep adding to our history:

```
git switch main
nano velocity.py         # on main: add a print line (with mistake - wrong unit)
```
```python
print("or", velocity_kmh, "m/s")
```
```
git diff
git add velocity.py
git commit -m "Print velocity on main (wrong unit)"
python velocity.py          # prints number with WRONG units
git switch b1
nano velocity               # add print statement with correct units
```
```python
print("or", velocity_kmh, "km/h")
```
```
git diff                    # 7 lines (7th line is new)
python velocity             # prints number with CORRECT units 
git add velocity.py
git commit -m "Print velocity on b1"
git lg                      # so many little changes
```

Maybe we are confused about what was done on which branch. Let's see how to investigate the changes.

```
git switch main
git diff                # changes from working dir to latest commit
git diff HEAD HEAD~1    # REMEMBER: changes between latest and one before latest commit
git diff HEAD~1 HEAD    # differences the other way around
git diff main b1        # changes between main and b1
git diff main b1~1      # changes between main and parent of the latest commit on branch b1
git diff main~1 b1~1    # changes between parent of the latest commit on main and parent of the latest commit on b1
                        # No difference on this one. Both files have the same content
```

## 11:05 - 3 💪 Explore differences across branches - 10' - TRAINER TWO

See `exercises.md`. There is an optional challenge.

Solution:

```
git switch b2
git diff HEAD
git diff HEAD~1
git diff HEAD~2
git diff HEAD~3
git diff HEAD~50        # fails -> unknown revision, our history is not that long
git diff b2 b1
git diff main b2~1
```

## 11:15 - Merging branches and conflict resolution - 15' - TRAINER TWO

We are done with developing on `b1` and want to merge to `main`. For this we first *stand* on main branch (with `switch`) and then apply the merge.

```
git switch main                     # on branch main
git diff main b1                    # verify
git merge -m "Merge b1" velocity.py # wrong syntax - merge expects branch not file
git merge -m "Merge b1" b1          # right syntax: fails because of conflict
git status                          # see which files are in conflict
git diff velocity.py                # verify differences
```

To resolve the merge conflict:

1. edit manually the conflicting sections: keep current branch (aka `ours`), incoming branch (aka `theirs`), combine or make entirely different changes.
2. remove the conflict markers

> **Reading the markers:**
>
> - `<<<<<<< HEAD` -> your current branch's version
> - `=======` -> dividing line
> - `>>>>>>>` -> the incoming branch's version

```
nano velocity.py         # edit the file within the conflict markers
                         # keep line with "km/h"
                         # demo ^k to delete whole line
```

`velocity.py` should look like this:

```
# Final velocity of an object after free falling during a given time
gravity = 9.81
time = 2.0
velocity = gravity * time
print("After", time, "s a free falling object reaches", velocity, "m/s")
velocity_kmh = velocity * 3.6
print("or", velocity_kmh, "km/h")
```
In this example, the conflict has been resolved by keeping the changes from branch `b1` (the line with the wrong `m/s` label is gone).

```
git status
git diff             # diff shows nothing! merge state - similar to staging
```

During a merge, conflicting files go to a different state while the conflict is resolved and a commit is done. The easiest way to see the state of the file is to show the content using `cat`.

```
python velocity.py                                       # now it runs
git add velocity.py                                      # mark resolution
git status                                               # verify
git commit -m "Merge changes from b1 into main"          # conclude merge 
git status                                               # verify
git lg                                                   # see merge visually
```

> **ADVANCED** If you really want to see the differences during a merge use: `git diff :1:velocity.py :2:velocity.py`

## 11:30 - 4 💪 A first type for merge and 5 💪 Undo a Bad Merge - 15' - TRAINER TWO

See `exercises.md`. There is an optional challenge under exercise 4.

Solution 4:

```
git diff main b2                # show differences all files
```
these two lines are on `main` but not on `b2`
```python
velocity_kmh = velocity * 3.6
print("or", velocity_kmh, "km/h")
```
git switch main                 # stand on main
git merge b2                    # merge b2 into main
nano velocity.py                # fix conflicts
```
<<<<<<< HEAD
velocity_kmh = velocity * 3.6
print("or", velocity_kmh, "km/h")
=======
velocity_kmh = 3.6 * velocity
>>>>>>> b2
```
The equation is equivalent, but it's easier to keep the whole change from `main`. Delete the marker lines.
```
git add velocity.py
git commit -m "Merge b2 into main"
git lg                          # verify
python velocity                 # it runs!
```


Solution 5:

```
git switch main
git branch bad-merge
git switch bad-merge
```
in `velocity.py` add
```python
print("under a gravity of", gravity, "km/s2")
```
```
git add velocity.py
git commit -m "Print gravity on bad-merge"
git switch main
```
in `velocity.py` add
```python
print("under a gravity of", gravity, "m/s2")
```
```
git add velocity.py
git commit -m "Print gravity on main"
git merge bad-merge          # conflict!
cat velocity.py             # conflict markers are visible
git status                  # velocity.py listed as "both modified"
git merge --abort
git status                  # clean - back to where you were before the merge
cat velocity.py             # conflict markers are gone, file is as it was on main
git lg                      # no merge commit was created
git branch -D bad-merge     # force-delete (it was never cleanly merged)
git lg                      # back to a clean main
```

`git merge --abort` is only available **while a merge is in progress**

> **Before break:** Turn to a neighbor and compare the output of `git lg`. Does it look the same? Different commit hashes? Same shape?

The top of the history should look like this (hashes will differ, and remote decorations depend on what you have pushed):

```
* d6565ee (HEAD -> main) Print gravity - correct units
*   41bccfb Merge b2 into main
|\  
| * f6a3c41 (b2) Convert velocity to km/h on b2
* |   834ba50 Merge changes from b1 into main
|\ \  
| * | 34c33d1 (b1) Print velocity on b1
| * | 9de488c Convert velocity to km/h on b1
| |/  
* | 1d1f992 Print velocity on main (wrong unit)
* | 8dd6d57 Convert velocity to km/h
|/  
* 9c793c1 (origin/main, origin/HEAD) Ignore log and data files
* 9bfc0aa Show result in human readable format
```

## 11:45 - Break - 15'

## 12:00 - 💻 PRACTICAL - Understanding merge conflicts - 30' - TRAINER ONE

TODO: rewrite using `distance.py` and `time.py`
see `PRACTICAL_merge_conflicts.md`

Do not solve the PRACTICAL live. Just ask questions, share experiences or highlight concepts that you noticed were still a bit confusing.

## 12:30 - Lunch - 60'

## 13:30 - Remote operations revisited - 10' - TRAINER TWO

🎥 Use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20version%20control%20with%20Git.pptx?d=w33d15b9f24e94794aaa7624d5b908dd3&csf=1&web=1&e=rgIzM0)

Explain remote operations:

- **Fetching**: downloading new data (commits, branches, or tags) from a remote repository into your local repository, without modifying your working files.
- **Pushing**: uploading your local commits to the remote repository.
- **Pulling**: `git pull` is `git fetch` + `git merge` in one step.

Explain **divergent branches**: both the local and remote `main` have moved forward independently. Git can try to reconcile the branches.

Explain the two most common options: `merge` and `rebase`.

## 13:40 - Remote operations live coding - 10' - TRAINER TWO

We made some local changes before lunch that are not pushed yet. So let's do that to ensure our remote repo is up to date

```
git push origin main
```

We confirm in GitHub that we see the changes. Notice that only the `main` branch was pushed. You can push other branches by specifying the branch name or by using the flag `--all`

```
git push origin b1
git push origin --all
```

Check in GitHub that you can see the branches.

Similarly, you can pull from a specific branch

```
git pull origin b1
```

If you want to sync all the branches from the remote to your local repo, you need to use `fetch`. Remember that `fetch` downloads the data but does not modify your files. It does not attempt to merge.

```
git fetch --all
```

## 13:50 - Solve a conflict when pushing - 10' - TRAINER TWO

Make a small edit directly on GitHub (via the web editor):

1. Open `velocity.py` on GitHub
2. Add a new line at the bottom: `print('Time:', time, 's')`
3. Commit with message `Add time print via GitHub`

Now, back in the terminal, make a **different** local change without pulling:

```
echo "print('Average velocity:', velocity / 2, 'm/s')" >> velocity.py
git add velocity.py
git commit -m "Print average velocity locally"
git push origin main        # fails! rejected - remote contains work you do not have
```

Let's ask Git to use `merge` by default

```
git config pull.rebase false      # merge
```

Resolve it:

```
git pull origin main        # fetch + attempt merge -> conflict in velocity.py
git status                  # velocity.py listed as "both modified"
nano velocity.py            # resolve conflict markers - here we keep both lines
git diff
git add velocity.py
git commit -m "Merge remote and local changes to velocity.py"
git push origin main        # now succeeds
```

Visit GitHub and confirm the resolved file is there.

> **Key message:** the conflict resolution steps are identical whether the divergence comes from a colleague or from your own edit on GitHub. **Pull before you push.**

## 14:00 - 6 💪 Rebase instead of merge - 15' - TRAINER TWO

Solution:

```
git pull origin main
git switch -c longer
nano velocity.py        # modify line 3 to `time = 10.0`
git diff
git add velocity.py
git commit -m "Increase time"
git switch main
nano velocity.py        # new last line `print('Calculation finished')`
git diff
git add velocity.py
git commit -m "Add closing message"
git switch longer
git rebase main        # Successfully rebased and updated refs/heads/longer
git lg                 # longer sits in front of main
```

> *DO NOT* clear screen here. So that participants can see the effect of rebase

```
git switch main
git merge longer          # Fast-forward
git lg
python velocity.py      # both changes are in the script: time is 4.0 and the closing message is printed
```

Notice that instead of a merge, Git just slides the `main` pointer up to match, like `fast-forwarding` a video to where you already know it ends.

> ⚠️ Rebase rewrites commit history. It is safe on a local branch that you have not shared with anyone. Avoid rebasing branches that have already been pushed.

## 14:15 - Break -15'

## 14:30 - 💻 PRACTICAL - Conflicts with Remote Repositories - 40' - TRAINER ONE

see `PRACTICAL_remote_conflicts.md`

Share experiences or highlight concepts that you noticed were still a bit confusing.

## 15:10 - VS Code demo - 10' - TRAINER ONE
==== continue here =====

Some people prefer to use a GUI to work with Git. Let's explore that using VSCode I will demo VS code using the `PRACTICAL | Understanding Merge Conflicts`

### Initialize repo

- Open VSCode
- Open folder -> make a new folder inside `Desktop` -> `conflict_practical`
- Explain the left tab to find the git menu
- Click on `Initialize Repository`
- Go to the file browser
- Make a new file `recipe.txt` and enter text from `PRACTICAL | Understanding Merge Conflicts`
  - You only need one file for this demo
  - Click on `recipe.txt` to see the changes on the right
  - red deleted
  - green added
  - Click on plus to stage
  - Write message and click on commit `Initial files`
- Notice the update on the log

### Branches and merge

- Follow the instruction to solve `💪 Conflict 1: Same-line edit`
  - Show how to switch branch + check out
  - When merging the first time select "no commit" to recreate the same as in the terminal
- Solve merge conflict
  - Explain incoming and current branch
  - Show the out of "Accept combination"
  - Show it can also be edited manually

## 15:20 - Summarize key points - 5' - TRAINER ONE

Using VS code

- **Branches**: create isolated lines of development with `git branch` and `git switch`.
- **Merging**: bring changes together with `git merge`. Git creates a merge commit
- **Conflicts**: happen when the same lines were changed in both branches. Always: edit -> remove markers -> `git add` -> `git commit`
- **Remote workflows**: `clone`, `push`, `pull`. Pull before you push. Conflicts can happen on remotes too, and are resolved the same way
- **Escape hatches**: if a conflict surprises you and you need time to think `git merge --abort` is a safe exit

## 15:25 - Give feedback about the course 5' - TRAINER ONE

Go to the link in `links.md`
