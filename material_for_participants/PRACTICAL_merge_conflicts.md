# PRACTICAL | Understanding Merge Conflicts


## Cheat sheet

| Command | What it does |
|-----------------------|---|
| `git merge --abort` | Cancel the merge entirely - restores everything to the pre-merge state|
| `git merge --quit` |  Forgets about the merge in progress but leaves your working tree and index exactly as they are. Afterward, your working tree may be a messy mix of both sides' changes, and Git won't remember that it was a merge. |
| `git reset --hard HEAD~1` | Equivalent of `undoing` that most recent commit |


## 💪 Conflict 1: Same-line edit

Alice and Bob both want to improve the wording of the **same** `print` line in `time.py`. Their edits are different, but both are good, so we will combine them.

**Step 1 — Alice's branch**

- Navigate to your `free_fall` local repository
- Create a new branch named `alice` and switch to it
- Open `time.py` for editing
- Modify the last line (the `print` line) so that `An object dropped from` becomes `A stone dropped from`. The line should now read:
```python
print("A stone dropped from", height, "m reaches the ground after", time_round, "s")
```
- Check the differences of file `time.py`
- Stage `time.py` and commit with the message `Alice: say stone instead of object`
- View the commit graph for all branches


<details>
<summary>🔍 Click here for hints! </summary>

- To create use `git branch name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the commit graph for all branches use `git log --oneline --all --graph` or the alias `git lg`
- To see the changes in a file use `git diff name_of_file`
- To stage a file use `git add name_of_file`
- To commit a file use `git commit -m "commit message"`
</details>

**Step 2 — Bob's branch**

- Bob branches from `main`, not from `alice` branch. So first, switch back to `main`
- View the commit graph for all branches
- Create a new branch named `bob` and switch to it
- Open `time.py` for editing
- Modify the last line (the `print` line) so that `reaches the ground after` becomes `hits the ground after`. The line should now read:
```python
print("An object dropped from", height, "m hits the ground after", time_round, "s")
```
- Check the differences of file `time.py`
- Stage `time.py` and commit with the message `Bob: say hits the ground`


<details>
<summary>🔍 Click here for hints! </summary>

- To create use `git branch name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the commit graph for all branches use `git log --oneline --all --graph` or the alias `git lg`
- To see the changes in a file use `git diff name_of_file`
- To stage a file use `git add name_of_file`
- To commit a file use `git commit -m "commit message"`
</details>

**Step 3 — Merge and see the conflict**

- Switch to `main`
- View the commit graph for all branches
- Merge `alice` into `main` (this should work fine)
- Merge `bob` into `main` (this should create a conflict)
- Open `time.py` with `nano` — you'll see:

```
<<<<<<< HEAD
print("A stone dropped from", height, "m reaches the ground after", time_round, "s")
=======
print("An object dropped from", height, "m hits the ground after", time_round, "s")
>>>>>>> bob
```

> **Reading the markers:**
> - `<<<<<<< HEAD` → your current branch's version
> - `=======` → dividing line
> - `>>>>>>>` → the incoming branch's version


<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To merge a branch into the current branch use `git merge incoming_branch`

</details>

**Step 4 — Resolve**

- Write one line that includes both improvements, e.g.:

```python
print("A stone dropped from", height, "m hits the ground after", time_round, "s")
```

- Delete all three marker lines (and the version of the `print` line you don't need), save, then:
> **Nano tip:** You can use `^K` to delete a whole line 

- Run `python time.py`. You should see `A stone dropped from 10 m hits the ground after 1.43 s`
- Check the differences of file `time.py`
- Stage `time.py` and commit with the message `Merge: combine Alice and Bob`
- View the commit graph for all branches. It should look like this: 

```bash
*   xxxxxxx (HEAD -> main) Merge: combine Alice and Bob
|\  
| * xxxxxxx (bob) Bob: say hits the ground
* | xxxxxxx (alice) Alice: say stone instead of object
|/  
* xxxxxxx Print gravity - correct units
```

## 💪 Conflict 2: Delete vs edit

One person deletes the comment at the top of `distance.py`, another person rewrites that same comment. Which change to keep?

**Step 1 — Create the two branches**

- Switch to `main`
- Create a new branch named `remove` and switch to it
- Open `distance.py` for editing
- Remove the first line (the comment). The content of `distance.py` should look like the text below:
```python
gravity = 1.62
time = 5
distance = 0.5 * gravity * time ** 2
print("After", time,"s the free fall distance on Moon is", distance, "m")
```
- Check the differences of file `distance.py`
- Stage `distance.py` and commit with the message `Remove comment`
- Switch to `main` to create a new branch
- Create a new branch named `rewrite` and switch to it
- Open `distance.py` for editing
- Modify the first line `# Distance that an object free falls during a given time` with `# Free fall distance using formula: d = 0.5 * g * t^2` and save
- Check the differences of file `distance.py`
- Stage `distance.py` and commit with the message `Rewrite comment to show the formula`
- View the commit graph for all branches


<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To create use `git branch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the changes in a file use `git diff name_of_file`
- To see the commit graph for all branches use `git log --oneline --all --graph` or the alias `git lg`

</details>

**Step 2 —  Merge and resolve**

- Switch to `main`
- Merge `remove` into `main` (this should be clean)
- Merge `rewrite` into `main` (this should create a conflict)
- Check the status of git. You should see something like:
```
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   distance.py
```
- Open `distance.py` with `nano` — notice that one side of the conflict is **empty**. That is how Git shows "this side deleted the line":

```
<<<<<<< HEAD
=======
# Free fall distance on the Moon: d = 0.5 * g * t^2
>>>>>>> rewrite
```

- Remove the comment entirely (keep no comment line), remove conflict markers and save
- Run `python distance.py` to check that the script works
- Stage `distance.py` and commit with the message `Merge: Confirm removal of comment`
- View the commit graph for all branches. It should look like this: 
```bash
*   xxxxxx (HEAD -> main) Merge: Confirm removal of comment
|\  
| * xxxxxxx (rewrite) Rewrite comment to show the formula
* | xxxxxxx (remove) Remove comment
|/  
*   xxxxxxx Merge: combine Alice and Bob
```

<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To merge branches, first stand on the branch that will *receive* the changes and then do the merge with `git merge incoming_branch`

</details>

**Step 3 —  Try the opposite**

Undo and resolve the other way:

- Undo the merge commit with a hard reset to the previous commit
- View the commit graph for all branches (confirm the merge commit is gone)
- Merge `rewrite` again to reproduce the conflict
- Open `distance.py` for editing

Imagine this code will be read by humans who are learning about free fall, so a comment with the formula is worth keeping.
- Keep `# Free fall distance using formula: d = 0.5 * g * t^2`, remove conflict markers and save
- Run `python distance.py` to check that the script works
- Stage `distance.py` and commit with the message `Merge: Keep comment with formula`
- View the commit graph for all branches. It should look like this: 
```bash
*   xxxxxx (HEAD -> main) Merge: Keep comment with formula
|\  
| * xxxxxxx (rewrite) Rewrite comment to show the formula
* | xxxxxxx (remove) Remove comment
|/  
*   xxxxxxx Merge: combine Alice and Bob
```


<details>
<summary>🔍 Click here for hints! </summary>

- To undo the latest commit (perform a hard reset) use `git reset --hard HEAD~1`

</details>


> **Take home message:** If you resolve this without thinking, you might permanently delete someone's work. In a real project, always ask why something was deleted before accepting "theirs."

## 💪 Conflict 3: Multi-file conflict

Two branches choose different planets. One of them changes **both** files, the other changes only one. Git will only complain about one file, but your intention is for both files to agree.

**Step 1 — Create the two branches**

- Switch to `main`
- Create a new branch named `jupiter` and switch to it
- Open `time.py`, modify gravity from `9.81` to `24.79` (Jupiter)
- Open `distance.py`, modify gravity from `1.62` to `24.79`
- Check the differences
- Stage `time.py` and `distance.py`
- Commit with the message `Use Jupiter gravity`
- Switch back to `main`
- Create a new branch named `moon` and switch to it
- Open `time.py`, modify gravity from `9.81` to `1.62` (Moon)
- Check the differences
- Stage `time.py` and commit with the message `Use Moon gravity in time.py`



<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To create use `git branch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the commit graph for all branches use `git log --oneline --all --graph` or the alias `git lg`

</details>

**Step 2 — Merge**

- Switch to `main`
- Merge `jupiter` into `main` (this should be clean)
- Merge `moon` into `main` (this should create a conflict in `time.py`)



<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To merge branches, first stand on the branch that will *receive* the changes and then do the merge with `git merge incoming_branch`

</details>

**Step 3 — Resolve, then check consistency**

The conflict is only in `time.py`, but `distance.py` now silently uses Jupiter gravity (`24.79`) while its `print` line still says *"on Moon"*. Whatever planet you choose, **all files must use the same gravity**.

- Open `time.py`, choose the Moon value `gravity = 1.62`, remove all conflict markers, save
- Open `distance.py`, update the gravity to that of the Moon (`1.62`)
- Run `python time.py`. You should see `A stone dropped from 10 m hits the ground after 3.51 s`
- Run `python distance.py`. You should see `After 5 s the free fall distance on Moon is 20.25 m`
- Stage both files and commit with the message `Merge: Use Moon gravity in all files`


Verify consistency:
Search both `time.py` and `distance.py` for the line containing `gravity =` and compare the results.

> **New command:** `grep` allows you to search files using text patterns.
```bash
grep 'gravity =' *.py
```

Expected output:
```
distance.py:gravity = 1.62
time.py:gravity = 1.62
velocity.py:gravity = 9.81
```
oh oh! we still have one inconsistency

**Step 3 — Use amend to fix inconsistency**

- Open `velocity.py`
- Change the value of gravity to that of the Moon (`1.62`)
- Run `python velocity.py`. You should see:
```
After 2.0 s a free falling object reaches 3.24 m/s
or 11.664000000000001 km/h
under a gravity of 1.62 m/s2
```
Let's **amend** the latest commit. Using the `--no-edit` flag we keep the previous commit message  `Merge: Use Moon gravity in all files` because it still makes sense. 

- Stage `velocity.py`
- Run `git commit --amend --no-edit`
- Use `git diff HEAD~1 HEAD` to see the changes that are included in the latest commit.You should see:
```
...distance.py 
-gravity = 24.79
+gravity = 1.62

...time.py
-gravity = 24.79
+gravity = 1.62

...velocity.py
-gravity = 9.81
+gravity = 1.62
```
-  If you see a `:` at the bottom of the screen, press `Enter` to continue the output. Once finished (e.g. you see `(END)`), press `q` to exit and go back to `bash`. 
- View the commit graph for all branches. It should look like this: 

```bash
*   xxxxxxx (HEAD -> main) Merge: Use Moon gravity in both files
|\  
| * xxxxxxx (moon) Use Moon gravity in time.py
* | xxxxxxx (jupiter) Use Jupiter gravity
|/  
*   xxxxxxx Merge: Keep comment with formula
```

## Reflection questions

- In Conflict 2, how did you decide whether to keep or remove the comment? What would make that easier in a real project?
- Why does Git pause and ask you to decide, rather than picking the most recent change?
- In Conflict 3, why is it a problem if `time.py`, `distance.py` or `velocity.py` use different gravity values?
- Git reported a conflict in only one file in Conflict 3. Why is a clean merge not always a *correct* merge?
