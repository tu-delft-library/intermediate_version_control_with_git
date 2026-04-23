# PRACTICAL | Understanding Merge Conflicts

## What is a merge conflict?

When two branches change the same part of the same file, Git can't guess which version is correct — so it stops and shows you both, and asks you to decide. That pause is a merge conflict. It's not an error; it's Git being careful.

> **Key insight:** A conflict means "two people changed the same thing — I need a human to decide." Git will never silently overwrite someone's work. That's a feature, not a bug.

---
## Escape hatches

| Command | What it does |
|---|---|
| `git merge --abort` | Cancel the merge entirely — safe at any time |
| `git status` | Show which files still have conflicts |
| `git diff` | Show all unresolved conflict content |
| `git switch --ours <file>` | Accept your branch's version of a file |
| `git switch --theirs <file>` | Accept the incoming branch's version |
| `git log --oneline` | Compact commit history |

---

## Overview

You'll play both sides of three conflicts using plain text files.

| # | Conflict type | File | What you decide |
|---|---|---|---|
| 1 | Same-line edit | `recipe.txt` | Combine both improvements |
| 2 | Delete vs edit | `bio.txt` | Keep or remove a paragraph |
| 3 | Multi-file | `event.txt` + `README.txt` | Keep both files consistent |

> **Remember:** If anything goes wrong, `git merge --abort` cancels the merge and takes you back to safety. Nothing touches your real files.

---

## Setup
> **Suggestion:** For this section, you are encouraged to copy/paste the contents of the files.

```bash
mkdir conflict_practical
cd conflict_practical
git init
nano recipe.txt
```
Add the text below to `recipe.txt`
```bash
Grandma's Tomato Soup
---------------------
Serves: 4
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
--------------
We are a small team of designers and writers based in Amsterdam.
We have been working together since 2019.
Our old office was in Rotterdam.
We moved to Amsterdam in 2021 for better transport links.
We specialise in brand identity and editorial design.
```
Confirm contents of `bio.txt`
Create a new file `event.txt`
```bash
cat bio.txt
nano event.txt
```
Add the text below to `event.txt`
```bash
Summer Workshop 2025
--------------------
Date: Saturday 14 June 2025
Location: Community Hall, Delft
Maximum attendees: 40
Ticket price: 25 euros
```
Confirm contents of `event.txt`
Create a new file `README.txt`
```bash
cat event.txt
nano README.txt
```
Add the text below to `README.txt`
```bash
Event details
-------------
Date: 14 June 2025
Ticket price: 25 euros
```
Confirm contents of `README.txt`
```bash
cat README.txt
git add .                           # . adds everything (only recommended for a first commit)
git commit -m "Initial files: recipe, bio, event, README"
```

> **Checkpoint:** `git log --oneline` should show one commit. `ls` should show all four files.

---

## Conflict 1: Same-line edit

> **Remember:** From here onwards, avoid copy pasting. Typing all the commands help you integrate your understanding.

**File:** `recipe.txt`

Alice adds a stirring note; Bob adds a seasoning reminder — to the same line.

**Step 1 — Alice's branch**

```bash
git switch -c alice
nano recipe.txt
```
Modify line `Add tomatoes and stock. Simmer for 25 minutes.` with `Add tomatoes and stock. Simmer for 25 minutes, stirring occasionally.`

Check the differences of file `recipe.txt` and commit
```bash
dif diff recipe.txt
git add recipe.txt
git commit -m "Alice: add stirring note to method"
git log --oneline -all --graph
```

**Step 2 — Bob's branch**

```bash
git switch main
git log --oneline -all --graph
git switch -c bob
nano recipe.txt
```
Modify line `Add tomatoes and stock. Simmer for 25 minutes.` with `Add tomatoes and stock. Season well. Simmer for 25 minutes.`

Check the differences of file `recipe.txt` and commit
```bash
git diff recipe.txt
git add recipe.txt
git commit -m "Bob: remind to season before simmering"
```

**Step 3 — Merge and see the conflict**

```bash
git switch main
git log --oneline -all --graph
git merge alice   # works fine
git merge bob     # creates a conflict
```

Open `recipe.txt` with `nano` — you'll see:

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

**Step 4 — Resolve**

Write one line that includes both improvements, e.g.:

```
Add tomatoes and stock. Season well. Simmer for 25 minutes, stirring occasionally.
```

Delete all three marker lines, save, then:
> **Nano tip:** You can use `^K` to delete a whole line 

Confirm the changes and commit
```bash
git diff recipe.txt
git add recipe.txt
git commit -m "Merge: combine Alice and Bob recipe improvements"
```

> **Success:** `recipe.txt` has no conflict markers, includes both the stirring note and seasoning step, and `git log --oneline -all --graph` shows a merge commit.

```bash
*   hash (HEAD -> main) Merge: combine Alice and Bob recipe improvements
|\  
| * hash (bob) Bob: remind to season before simmering
* | hash (alice) Alice: add stirring note to method
|/  
* hash Initial files: recipe, bio, event, README
```
---

## Conflict 2: Delete vs edit


**File:** `bio.txt`

One person deletes the Rotterdam paragraph (outdated); another rewrites it to sound warmer — without knowing it was deleted.

> **Warning:** If you resolve this without thinking, you might permanently delete someone's work. In a real project, always ask why something was deleted before accepting "theirs."

**Step 1 — Create the two branches**

```bash
git switch main
git switch -c remove
nano bio.txt
```
Remove lines referring to Rotterdam office (i.e. third and fourth lines).
The content of `bio.txt` should like the text below:
```bash
About Our Team
--------------
We are a small team of designers and writers based in Amsterdam.
We have been working together since 2019.
We specialise in brand identity and editorial design.
```
Check changes and commit
```bash
git diff bio.txt
git add bio.txt
git commit -m "Remove outdated Rotterdam office paragraph"
```
Go back to `main`
```bash
git switch main
git log --oneline --all --graph
git switch -c rewrite
nano bio.txt
```
Modify the line `Our old office was in Rotterdam.` with `We started out in Rotterdam, which we loved.`

Check changes and commit
```bash
git diff bio.txt
git add bio.txt
git commit -m "Rewrite Rotterdam history to sound more personal"
```

**Step 2 — Trigger the conflict**

```bash
git switch main
git merge remove   # clean
git merge rewrite  # conflict
```

**Step 3 — Decide and resolve**

For this exercise: imagine a client associates the team with Rotterdam, so the history is worth keeping. 

Edit `bio.txt` using `nano`. Keep the warmer rewrite, remove conflict markers, then:

```bash
git add bio.txt
git commit -m "Keep Rotterdam history with warmer wording"
git log --oneline --all --graph
```

Now try the opposite — undo and resolve the other way:

```bash
git reset HEAD~1                    # undo the merge commit
git log --oneline --all --graph     # see the merge commit is removed
git merge rewrite                   # do the merge again and get a conflict
git nano bio.txt
```
Remove the Rotterdam paragraph entirely, remove conflict markers, then:
```bash
git add bio.txt
git commit -m "Confirm removal of Rotterdam paragraph"
git log --oneline --all --graph
```

> **Success:** `bio.txt` has no conflict markers, reads naturally, and you can explain in one sentence why you made your choice.

---

## Conflict 3: Multi-file conflict

**Files:** `event.txt` + `README.txt`

Two people update the ticket price in `event.txt`, but only one also updates `README.txt`. After merging, the conflict in `event.txt` is visible — but the stale price in `README.txt` is a hidden inconsistency.

**Step 1 — Create the two branches**

```bash
git switch main
git switch -c raise
```
- Open `event.txt` file with `nano`
- Modify ticket price from `25 euros` to `35 euros`
- Do the same for file `README.txt`
- Confirm changes and commit
```bash
git diff
git add event.txt README.txt
git commit -m "Raise ticket price to 35 euros (covers catering)"

git switch main
git switch -c lower
```
- Open `event.txt` file with `nano`
- Modify ticket price from `25 euros` to `15 euros`
- Confirm changes and commit
```bash
git diff
git add event.txt
git commit -m "Lower ticket price to 15 euros (increase accessibility)"
```

**Step 2 — Trigger the conflict**

```bash
git switch main
git merge raise   # clean
git merge lower   # conflict in event.txt
```

**Step 3 — Resolve, then check consistency**

The conflict is only in `event.txt`, but `README.txt` now shows 35 euros. Whatever price you choose, **both files must match**.

1. Open `event.txt`, choose a price (either value, or a compromise like 20 euros), remove all conflict markers, save.
1. Open `README.txt`, update the price to match exactly.
1. Check the differences
1. Stage both files and commit:

```bash
git diff
git add event.txt README.txt
git commit -m "Resolve ticket price conflict: set to 20 euros in both files"
```

Verify consistency:
> **New command:** `grep` allows you to search files using text patterns.
```bash
grep 'Ticket price' event.txt README.txt
```

> **Success:** No conflict markers in `event.txt`, both files show the same price, `git status` is clean, and `git log --oneline --all --graph` shows three merge commits.

---

## Bonus challenge

Make a mistake on purpose, then undo it safely.

```bash
# Delete the Ingredients section from recipe.txt, then commit the mistake
git add recipe.txt
git commit -m "oops: accidentally deleted ingredients"

# Undo it safely — adds a fix commit without erasing history
git revert --no-commit HEAD
git commit -m "Reinstating ingredients"
git log --oneline --all --graph
```

> `git revert` is safe when you've already pushed to remote. If you haven't pushed yet, `git reset HEAD~1` removes the commit entirely.

---



## Reflection questions

- In Conflict 2, how did you decide whether to keep or remove the Rotterdam paragraph? What would make that easier in a real project?
- Why does Git pause and ask you to decide, rather than picking the most recent change?
- In Conflict 3, why is it a problem if `event.txt` and `README.txt` show different prices?
- What's the difference between `git revert` and deleting a commit? When would you use each?

---

