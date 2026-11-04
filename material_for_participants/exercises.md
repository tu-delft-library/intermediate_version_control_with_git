# Exercises


## 1 💪 Get familiar with branches

Follow the steps listed below:

- Create two branches with any name you like
- Create a new branch and switch in one step
- Rename them and switch between them
- Check the status message and the history in each branch you are in

When you are finished:
- If you have a branch `master`, rename it to `main`
- Return to the branch `main`
- Delete all other branches

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

- Create a branch called `moon` and switch to it
- Open `velocity.py` in nano
- Modify line 2 to `gravity = 1.62`
- Run the script. You should see the text `After 2.0 s ... 3.25 m/s`
- Commit your changes with the message `Change to Moon gravity`
- Switch back to main
- Try to delete `moon` with `git branch -d moon`
- Git will refuse. Read the error message carefully — why is it blocking you?
- Follow the hint in the error message to force delete the branch


<details>
<summary>🔍 Explanation! </summary>

- `-d` is a safe delete — Git refuses if the branch has commits that have not been merged into the current branch. It is protecting you from losing work.
- `-D` is a force delete — Git deletes it regardless. The commits are not immediately gone but they are hard to recover.

>**As a rule of thumb:** if `-d` refuses, pause and ask yourself whether those commits are actually needed before forcing with `-D`
</details>


## 2 💪 Commit on a secondary branch

Follow the steps listed below:

- Move to branch `b2` 
- Open `velocity.py` using nano
- Confirm that the file `velocity.py` looks like this:
```
# Final velocity of an object after free falling during a given time
gravity = 9.81
time = 2.0
velocity = gravity * time
print("After", time, "s a free falling object reaches", velocity, "m/s")
```

- Add this line to the file
```python
velocity_kmh = 3.6 * velocity
```
- Commit your changes. Specify the branch name in the commit message!
- Inspect the working tree


<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To inspect the differences use `git diff`
- To inspect the working tree use `git status`
</details>


## 3 💪 Explore differences across branches

Follow the steps listed below:

- Move to branch `b2`
- Inspect several previous versions in the history of `b2` using `git diff` with appropriate arguments
- How far back in the history of branch `b2` can you go?
    - If you see `:` it means there is more content waiting to be shown. 
    - Click `Enter` to continue scrolling. 
    - When you reach the end, press `q` to exit
- Inspect the differences between the latest commit of branch `b2` and branch `b1`
- Inspect the differences between the `main` branch and the parent of the latest commit on branch `b2`

<details>
<summary>🔍 Click here for hints! </summary>


- To switch between branches use `git switch name_of_branch`
- To inspect different versions in the history use in the current branch use `git diff HEAD~1`
- To inspect differences between branches `git diff name_of_branch name_of_another_branch`
- Add `~1` or `~2` to the name of the branch to refer to earlier commits (e.g. parents)
</details>

#### 🚀 Optional challenge — detect a specific change

- Use `git log --oneline --all` to list the commits across all branches
- Use `git diff <hash1> <hash2>` with pairs of commit hashes to narrow down which commit first introduced `fifth line`
- Confirm your answer with `git show <hash>` — this shows the full content of a single commit


## 4 💪 A first type of merge

Follow the steps listed below:

- Inspect the differences of `velocity.py` between branches. What lines are there in `velocity.py` in `main` but not in `b2`? 
- Merge `b2` into `main`
- Solve any conflicts. The equation is equivalent, but it's easier to keep the whole change from `main`
- Commit the merge
- Verify the outcome with a graphed `git log`
- Run the script. You should see the text `After 2.0 s ... 19.62 m/s or 70.632 km/h`
- With the merge complete and the script running, you can delete the branch `b2`

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

- Make sure you are on the `main` branch
- Create a new branch called `bad-merge` and step into it
- Open `velocity.py` with nano
- Add the line
```python
print("under a gravity of", gravity, "km/s2")
```
- Run the script. You should see the new line `under a gravity of 9.81 km/s2`
- Commit your changes with the message `Print gravity - wrong units`
- Switch back to `main`
- Open `velocity.py` with nano
- Add the line
```python
print("under a gravity of", gravity, "m/s2")
```
- Run the script. You should see the new line `under a gravity of 9.81 m/s2`
- Commit your changes with the message `Print gravity - correct units`
- Merge `bad-merge` branch into `main`
- Use `cat` to see the content of `velocity.py`
- Check the status of git. You should see something like:
```
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   velocity.py
```
- You notice the changes from `bad-merge` do not make sense. So you decide to take a step back.
- Use the command `git merge --abort` to cancel the merge process
- Check the status of git
- Use `cat` to see the content of `velocity.py`
- Check the history of git and confirm there was no merge commit created
- Force delete the `bad-merge` branch using `git branch -D bad-merge`

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

Let's make the output more readable. We will round the value stored in the variable `velocity_kmh` to have only 2 decimals. We used this operation in the `PRACTICAL_collaboration.md` in the last session.

- Pull the latest `main` from remote
- Create a new branch called `round` from `main` and switch to it
- Open `velocity.py` and change `velocity_kmh = round(velocity * 3.6, 2)`
- Run the script. You should see
```
After 2.0 s a free falling object reaches 3.24 m/s
or 11.66 km/h
under Moon gravity of 1.62 m/s2
```
- Stage and commit it with the message `Round velocity_kmh to have 2 decimals`
- Switch back to `main`
- You decide the ensure the variable `velocity` also has two decimals
- Open `velocity.py` and change `velocity = round(gravity * time, 2)`
- Run `velocity.py`. You should see
```python
After 2.0 s a free falling object reaches 3.24 m/s
or 11.664000000000001 km/h
under Moon gravity of 1.62 m/s2
```
- Stage and commit it with the message `Round velocity to have 2 decimals`
- Inspect the result with `git lg` (or the longer version `git log --oneline --all --graph`).
- Notice that the two branches (`main` and `round`) have now diverged
- Switch back to `round` and run `git rebase main`. You should see:
```
Successfully rebased and updated refs/heads/round.
```
- Inspect the result with `git lg` (or the rebase version `git log --oneline --all --graph`)

    Compare what you see with the graph from the previous merges:
    - Where does `round` sit relative to `main`?
    - What happened to the original commit on `round`?

- Switch to `main` and merge `round` with `git merge round`
- Check the graph again — notice that instead of a merge, Git just slides the `main` pointer up to match, like `fast-forwarding` a video to where you already know it ends.

> ⚠️ Rebase rewrites commit history. It is safe on a local branch that you
> have not shared with anyone. Avoid rebasing branches that have already been pushed.


## 7 🚀 Optional: Interactive Git 

- Go to this link https://learngitbranching.js.org/
- Play with your favorite episode
