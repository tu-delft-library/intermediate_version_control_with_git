# PRACTICAL | Understanding Merge Conflicts

## What is a merge conflict?

When two branches change the same part of the same file, Git can't guess which version is correct — so it stops and shows you both, and asks you to decide. That pause is a merge conflict. It's not an error; it's Git being careful.

A conflict means "two people changed the same thing — I need a human to decide." Git will never silently overwrite someone's work. That's a feature, not a bug.

## Escape hatches

| Command | What it does |
|-----------------------|---|
| `git merge --abort` | Cancel the merge entirely - restores everything to the pre-merge state|
| `git merge --quit` |  Forgets about the merge in progress but leaves your working tree and index exactly as they are. Afterward, your working tree may be a messy mix of both sides' changes, and Git won't remember that it was a merge. |
| `git reset --hard HEAD~1` | Equivalent of `undoing` that most recent commit |


## Setup
For this section, and only for this section, you are encouraged to **copy/paste the contents of the files**.

```bash
cd ~/Desktop
mkdir conflict_practical
cd conflict_practical
git init
nano recipe.txt
```
Add the text below to `recipe.txt`
```bash
Grandma's Tomato Soup
------------------Serves: 4
Prep time: 10 minutes
Cook time: 30 minutes

Ingredients:
- 6 ripe tomatoes
- 1 onion, chopped
- 2 cloves garlic
- 500ml vegetable stock
- Salt and pepper to taste

Method:
Fry the onion and garlic until soft.
Add tomatoes and stock. Simmer for 25 minutes.
Blend until smooth. Season and serve.
```
Confirm contents of `recipe.txt`
Create a new file `bio.txt`
```bash
cat recipe.txt
nano bio.txt
```
Add the text below to `bio.txt`
```bash
About Our Team
-----------
We are a small team based in Amsterdam.
We have been running catering events since 2019.
Our old office was in Delft.
We moved to Amsterdam in 2021 for better transport links.
We specialise in traditional italian.
```
Confirm contents of `bio.txt`
Create a new file `event.txt`
```bash
cat bio.txt
nano event.txt
```
Add the text below to `event.txt`
```bash
Summer Festival 2025
-----------------
Date: Saturday 14 June 2025
Location: Community Hall, Delft
Maximum attendees: 40
Meal price: 25 euros
```
Confirm contents of `event.txt`
Create a new file `summary.txt`
```bash
cat event.txt
nano summary.txt
```
Add the text below to `summary.txt`
```bash
Event details
----------
Date: 14 June 2025
Meal price: 25 euros
```
Confirm contents of `summary.txt`
```bash
cat summary.txt
git add .                           # . adds everything (only recommended for a first commit)
git commit -m "Initial files"
```

> **Checkpoint:** `git log --oneline` should show one commit. `ls` should show all four files.


## 💪 Conflict 1: Same-line edit


> **Situation** File `recipe.txt`; Alice adds a stirring note; Bob adds a seasoning reminder — to the same line.


**Step 1 — Alice's branch**

- Create a new branch named `alice` and switch to it
- Open `recipe.txt` for editing
- Modify line `Add tomatoes and stock. Simmer for 25 minutes.` with `Add tomatoes and stock. Simmer for 25 minutes, stirring occasionally.`
- Check the differences of file `recipe.txt`
- Stage `recipe.txt` and commit with the message `Alice: add stirring note`
- View the commit graph for all branches


<details>
<summary>🔍 Click here hints! </summary>

- To create use `git branch name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the commit graph for all branches use `git log --oneline --all --graph`
- To see the changes in a file use `git diff name_of_file`
- To stage a file use `git add name_of_file`
- To commit a file use `git commit -m "commit message"`
</details>

**Step 2 — Bob's branch**

- Bob branches from `main`, not from `alice` branch. So first, switch back to `main`
- View the commit graph for all branches
- Create a new branch named `bob` and switch to it
- Open `recipe.txt` for editing
- Modify line `Add tomatoes and stock. Simmer for 25 minutes.` with `Add tomatoes and stock. Season well. Simmer for 25 minutes.`
- Check the differences of file `recipe.txt`
- Stage `recipe.txt` and commit with the message `Bob: remind to season`


<details>
<summary>🔍 Click here hints! </summary>

- To create use `git branch name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the commit graph for all branches use `git log --oneline --all --graph`
- To see the changes in a file use `git diff name_of_file`
- To stage a file use `git add name_of_file`
- To commit a file use `git commit -m "commit message"`
</details>

**Step 3 — Merge and see the conflict**

- Switch to `main`
- View the commit graph for all branches
- Merge `alice` into `main` (this should work fine)
- Merge `bob` into `main` (this should create a conflict)
- Open `recipe.txt` with `nano` — you'll see:

```
<<<<<<< HEAD (your version)
Add tomatoes and stock. Simmer for 25 minutes, stirring occasionally.
=======
Add tomatoes and stock. Season well. Simmer for 25 minutes.
>>>>>>> bob (their version)
```

> **Reading the markers:**
> - `<<<<<<< HEAD` → your current branch's version
> - `=======` → dividing line
> - `>>>>>>>` → the incoming branch's version


<details>
<summary>🔍 Click here hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To merge a branch into the current branch use `git merge incoming_branch`

</details>

**Step 4 — Resolve**

- Write one line that includes both improvements, e.g.:

```
Add tomatoes and stock. Season well. Simmer for 25 minutes, stirring occasionally.
```

- Delete all three marker lines, save, then:
> **Nano tip:** You can use `^K` to delete a whole line 

- Check the differences of file `recipe.txt`
- Stage `recipe.txt` and commit with the message `Merge: combine Alice and Bob`


> **Success:** `recipe.txt` has no conflict markers, includes both the stirring note and seasoning step, and the commit graph shows a merge commit.

```bash
*   hash (HEAD -> main) Merge: combine Alice and Bob
|\  
| * hash (bob) Bob: remind to season
* | hash (alice) Alice: add stirring note
|/  
* hash Initial files
```

## 💪 Conflict 2: Delete vs edit


> **Situation:** File `bio.txt`; One person deletes the Delft paragraph (outdated); another rewrites it to sound warmer — without knowing it was deleted.


**Step 1 — Create the two branches**

- Switch to `main`
- Create a new branch named `remove` and switch to it
- Open `bio.txt` for editing
- Remove lines referring to Delft office (i.e. third and fourth lines). The content of `bio.txt` should look like the text below:
```bash
About Our Team
-----------
We are a small team based in Amsterdam.
We have been running catering events since 2019.
We specialise in traditional italian.
```
- Check the differences of file `bio.txt`
- Stage `bio.txt` and commit with the message `Remove Delft office paragraph`
- Switch to `main` to create a new branch
- Create a new branch named `rewrite` and switch to it
- Open `bio.txt` for editing
- Modify the line `Our old office was in Delft.` with `We started out in Delft, which we loved.` and save
- Check the differences of file `bio.txt`
- Stage `bio.txt` and commit with the message `Rewrite Delft history to sound more personal`
- View the commit graph for all branches


<details>
<summary>🔍 Click here hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To create use `git branch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the changes in a file use `git diff name_of_file`
- To see the commit graph for all branches use `git log --oneline --all --graph`

</details>

**Step 2 — Trigger the conflict**

- Switch to `main`
- Merge `remove` into `main` (this should be clean)
- Merge `rewrite` into `main` (this should create a conflict)


<details>
<summary>🔍 Click here hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To merge a branch into the current branch use `git merge incoming_branch`

</details>

**Step 3 — Decide and resolve**

Imagine a client associates the team with Delft, so the history is worth keeping. 

- Open `bio.txt` for editing
- Keep `We started out in Delft, which we loved.`, remove conflict markers and save
- Stage `bio.txt` and commit with the message `Merge: Keep Delft history with warmer wording`
- View the commit graph for all branches

Now try the opposite — undo and resolve the other way:

- Undo the merge commit with a hard reset to the previous commit
- View the commit graph for all branches (confirm the merge commit is gone)
- Merge `rewrite` again to reproduce the conflict
- Open `bio.txt` for editing
- Remove the Delft paragraph entirely, remove conflict markers and save
- Stage `bio.txt` and commit with the message `Merge: Confirm removal of Delft paragraph`
- View the commit graph for all branches



<details>
<summary>🔍 Click here hints! </summary>

- To undo the latest commit (perform a hard reset) use `git reset --hard HEAD~1`
- To switch between branches use `git switch name_of_branch`
- To merge a branch into the current branch use `git merge incoming_branch`

</details>


> **Success:** `bio.txt` has no conflict markers, reads naturally, and you can explain in one sentence why you made your choice.


> **Warning:** If you resolve this without thinking, you might permanently delete someone's work. In a real project, always ask why something was deleted before accepting "theirs."

## 💪 Conflict 3: Multi-file conflict

> **Situation:** Files `event.txt` + `summary.txt`; Two people update the meal price in `event.txt`, but only one also updates `summary.txt`. After merging, the conflict in `event.txt` is visible — but the stale price in `summary.txt` is a hidden inconsistency.

**Step 1 — Create the two branches**

- Switch to `main`
- Create a new branch named `raise` and switch to it
- Open `event.txt`, modify meal price from `25 euros` to `35 euros`
- Do the same for `summary.txt`
- Check the differences
- Stage `event.txt` and `summary.txt`
- Commit with the message `Raise meal price to cover drinks`
- Switch back to `main`
- Create a new branch named `lower` and switch to it
- Open `event.txt`, modify meal price from `25 euros` to `15 euros`
- Check the differences
- Stage `event.txt` and commit with the message `Lower meal price for accessibility`



<details>
<summary>🔍 Click here hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To create use `git branch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To see the commit graph for all branches use `git log --oneline --all --graph`

</details>

**Step 2 — Trigger the conflict**

- Switch to `main`
- Merge `raise` into `main` (this should be clean)
- Merge `lower` into `main` (this should create a conflict in `event.txt`)



<details>
<summary>🔍 Click here hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To merge a branch into the current branch use `git merge incoming_branch`

</details>

**Step 3 — Resolve, then check consistency**

The conflict is only in `event.txt`, but `summary.txt` now shows 35 euros. Whatever price you choose, **both files must match**.

- Open `event.txt`, choose a compromise price of 20 euros, remove all conflict markers, save
- Open `summary.txt`, update the price to match exactly
- Check the differences
- Stage both files and commit with the message `Merge: Set to 20 euros in both files`


Verify consistency:
Search both `event.txt` and `summary.txt` for the line containing `Meal price` and compare the results.

> **New command:** `grep` allows you to search files using text patterns.
```bash
grep 'Meal price' *.txt
```

> **Success:** No conflict markers in `event.txt`, both files show the same price, the working tree is clean, and the commit graph shows three merge commits.


## 🚀 Optional challenge

Make a mistake on purpose, then undo it safely.

- Delete the Ingredients section from `recipe.txt`
- Stage `recipe.txt` and commit with the message `oops: accidentally deleted ingredients`
- Undo the mistake without erasing history — this should add a new commit that reverses the previous one, staged but not yet committed
- Commit the reversal with the message `Reinstating ingredients`
- View the commit graph for all branches

<details>
<summary>🔍 Click here hints! </summary>

- To undo a commit use `git revert --no-commit HEAD`
</details>

> Reversing a commit this way is safe when you've already pushed to remote. If you haven't pushed yet, a hard reset to the previous commit removes the commit entirely.


## Reflection questions

- In Conflict 2, how did you decide whether to keep or remove the Delft paragraph? What would make that easier in a real project?
- Why does Git pause and ask you to decide, rather than picking the most recent change?
- In Conflict 3, why is it a problem if `event.txt` and `summary.txt` show different prices?
- What's the difference between reverting a commit and deleting a commit? When would you use each?


