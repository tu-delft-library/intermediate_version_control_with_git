# Exercises


## 1 💪 Get familiar with branches

Follow the steps listed below:

1. Create two branches with any name you like
1. Create a new branch and switch in one step
1. Rename them and switch between them
1. Check the status message and the history in each branch you are in

When you are finished:
1. If you have a branch `master`, rename it to `main`
1. Return to the branch `main`
1. Delete all other branches

<details>
<summary>🔍 Click here for hints! </summary>

- To create use `git branch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To rename use `git branch -m current_name_of_branch new_name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To check the history of a branch use `git log --oneline`
- To delete branches use `git branch -d name_of_branch name_of_another_branch`
</details>

#### 🚀 Optional challenge — safe delete vs force delete

1. Create a branch called `moon` and switch to it
1. Open `velocity.py` in nano
1. Modify line 2 to `gravity = 1.62`
1. Run the script. You should see the text `After 2.0 s ... 3.25 m/s`
1. Commit your changes with the message `Change to Moon gravity`
1. Switch back to main
1. Try to delete `moon` with `git branch -d moon`
1. Git will refuse. Read the error message carefully — why is it blocking you?
1. Follow the hint in the error message to force delete the branch


<details>
<summary>🔍 Explanation! </summary>

- `-d` is a safe delete — Git refuses if the branch has commits that have not been merged into the current branch. It is protecting you from losing work.
- `-D` is a force delete — Git deletes it regardless. The commits are not immediately gone but they are hard to recover.

>**As a rule of thumb:** if `-d` refuses, pause and ask yourself whether those commits are actually needed before forcing with `-D`
</details>


## 2 💪 Commit on a secondary branch

Follow the steps listed below:

1. Move to branch `b2` 
1. Open `velocity.py` using nano
1. Confirm that the file `velocity.py` looks like this:
```
# Final velocity of an object after free falling during a given time
gravity = 9.81
time = 2.0
velocity = gravity * time
print("After", time, "s a free falling object reaches", velocity, "m/s")
```

1. Add this line to the file
```python
velocity_kmh = velocity * 3.6
```
1. Commit your changes. Specify the branch name in the commit message!
1. Inspect the working tree


<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To inspect the differences use `git diff`
- To inspect the working tree use `git status`
</details>


## 3 💪 Explore differences across branches

Follow the steps listed below:

1. Move to branch `b2`
1. Inspect several previous versions in the history of `b2` using `git diff` with appropriate arguments
1. How far back in the history of branch `b2` can you go?
    - If you see `:` it means there is more content waiting to be shown. 
    - Click `Enter` to continue scrolling. 
    - When you reach the end, press `q` to exit
1. Inspect the differences between the latest commit of branch `b2` and branch `b1`
1. Inspect the differences between the `main` branch and the parent of the latest commit on branch `b2`

<details>
<summary>🔍 Click here for hints! </summary>


- To switch between branches use `git switch name_of_branch`
- To inspect different versions in the history use in the current branch use `git diff HEAD~1`
- To inspect differences between branches `git diff name_of_branch name_of_another_branch`
- Add `~1` or `~2` to the name of the branch to refer to earlier commits (e.g. parents)
</details>

#### 🚀 Optional challenge — detect a specific change

1. Use `git log --oneline --all` to list the commits across all branches
1. Use `git diff <hash1> <hash2>` with pairs of commit hashes to narrow down which commit first introduced `fifth line`
1. Confirm your answer with `git show <hash>` — this shows the full content of a single commit


## 4 💪 A first type of merge

Follow the steps listed below:

1. Inspect the differences of `velocity.py` between branches. What lines are there in `velocity.py` in `main` but not in `b2`? 
1. Merge `b2` into `main`
1. Solve any conflicts. The equation is equivalent, but it's easier to keep the whole change from `main`
1. Commit the merge
1. Verify the outcome with a graphed `git log`

<details>
<summary>🔍 Click here for hints! </summary>

- To inspect differences between branches `git diff name_of_branch name_of_another_branch`
- To merge branches, first stand on the branch that will *receive* the changes with `git switch target_branch` and then do the merge with `git merge incoming_branch`
- Use `nano file_in_conflict` to edit the file manually
</details>

#### 🚀 Optional challenge — read the graph

After the merge is done, look at the graph with `git log --oneline --all --graph`.  
Now try to answer these questions just by reading the graph — without using `cat` or `diff`:

- Which commit introduced the conflict?
- Which branch was ahead before the merge?

Then verify your answers by inspecting the commits directly with
`git diff <hash1> <hash2>`.


## 5 💪 Undo a bad merge

Merges don't always go as planned. In this exercise you will practice how to cancel a merge while it is in progress.

1. Create a new branch called `bad-merge` and step into it
1. Open `velocity.py` with nano
1. Add the line
```python
print("under a gravity of", gravity, "km/s2")
```
1. Run the script. You should see the new line `under a gravity of 9.81 km/s2`
1. Commit your changes with the message `Print gravity - wrong units`
1. Switch back to `main`
1. Open `velocity.py` with nano
1. Add the line
```python
print("under a gravity of", gravity, "m/s2")
```
1. Run the script. You should see the new line `under a gravity of 9.81 m/s2`
1. Commit your changes with the message `Print gravity - correct units`
1. Merge `bad-merge` branch into `main`
1. Use `cat` to see the content of `velocity.py`
1. Check the status of git. You should see something like:
```
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   velocity.py
```
1. You notice the changes from `bad-merge` do not make sense. So you decide to take a step back.
1. Use the command `git merge --abort` to cancel the merge process
1. Check the status of git
1. Use `cat` to see the content of `velocity.py`
1. Check the history of git and confirm there was no merge commit created
1. Force delete the `bad-merge` branch using `git branch -D bad-merge`

<details>
<summary>🔍 Click here for hints! </summary>

- To create use `git branch name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To merge branches, first stand on the branch that will *receive* the changes with `git switch target_branch` and then do the merge with `git merge incoming_branch`
- To check the status of git use `git status`
- To check the history of a branch use `git log --oneline --all --graph` or the alias `git lg`
</details>



## 6 💪 Rebase instead of merge
We can use rebase when the changes touch different files or clearly separate parts of a file. This way the rebase is a clean, non-conflicting replay of `history`.

1. Pull the latest `main` from remote
1. Create a new branch called `longer` from `main` and switch to it in one step
1. Open `velocity.py` and modify line 3 to `time = 4.0`
1. Run the script. You should see `After 4.0 ... 39.24 m/s`
1. Stage and commit it with the message `Increase time`
1. Switch back to `main` 
1. Open `velocity.py` and add a new line :
```python
print('Calculation finished')
```
1. Run the script. You should see the last line `Calculation finished`
1. Stage and commit it with the message `Add closing message`
1. Inspect the result with `git lg` (or the longer version `git log --oneline --all --graph`).
1. Notice that the two branches (`main` and `longer`) have now diverged
1. Switch back to `longer` and run `git rebase main`. You should see:
```
Successfully rebased and updated refs/heads/longer.
```
1. Inspect the result with `git lg` (or the longer version `git log --oneline --all --graph`)

    Compare what you see with the graph from the previous merges:
    - Where does `longer` sit relative to `main`?
    - What happened to the original commit on `longer`?

1. Switch to `main` and merge `longer` with `git merge longer`
1. Check the graph again — notice that instead of a merge, Git just slides the `main` pointer up to match, like `fast-forwarding` a video to where you already know it ends.

> ⚠️ Rebase rewrites commit history. It is safe on a local branch that you
> have not shared with anyone. Avoid rebasing branches that have already been pushed.


## 7 🚀 Optional: Interactive Git 

1. Go to this link https://learngitbranching.js.org/
1. Play with your favorite episode
