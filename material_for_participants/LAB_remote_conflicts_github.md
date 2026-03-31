# Lab | Conflicts with Remote Repositories | ~35 min

## What is a remote conflict?

When you and a colleague both change the same file and try to share your work through a shared remote repository, Git will stop and ask you to sort it out. This happens in two situations:

- **Push rejected** — you try to upload your work, but someone else pushed first. Git won't overwrite their changes.
- **Pull conflict** — you download someone else's changes, but they clash with edits you've already made locally.

> **Key insight:** Git will never silently overwrite work. A rejected push or a pull conflict is Git protecting your team's work — not a sign that something has broken.

---

## Escape hatches

| Command | What it does |
|---|---|
| `git merge --abort` | Cancel a merge in progress — safe at any time |
| `git status` | Show which files still have conflicts |
| `git diff` | Show all unresolved conflict content |
| `git log --oneline --graph` | Compact history showing branches and merges |
| `git pull origin main` | Fetch and merge the remote's latest changes |
| `git push origin main` | Upload your commits to the remote |
| `git fetch origin` | Download remote changes without merging yet |

---

## Lab overview

You'll simulate two people working on the same GitHub repository. You'll play both sides by using two local clones of the same repo.

| # | Conflict type | File | What you decide |
|---|---|---|---|
| 1 | Push rejected | `notes.txt` | Merge before you can push |
| 2 | Pull conflict | `schedule.txt` | Resolve clashing edits after pulling |
| 3 | Diverged history | `ideas.txt` | Decide what both versions keep |

> **Remember:** `git merge --abort` cancels a merge in progress and returns everything to how it was. `git pull` is just `git fetch` + `git merge` in one step.

---

## Setup

You'll create one repository on GitHub and clone it twice — once as "you" and once as a "colleague". Both clones share the same GitHub remote, just like a real team would.

**Step 1 — Create the repository on GitHub**

1. Go to [github.com](https://github.com) and sign in.
2. Click the **+** icon in the top-right corner and choose **New repository**.
3. Name it `remote_conflicts_lab`.
4. Leave it **Public** (or Private — either works).
5. Leave all clone_colleague options as they are. Do **not** add a README or .gitignore.
6. Click **Create repository**.

GitHub will show you your new empty repository and its URL. 
Click on the `SSH` tab. It will look like:
```
git@github.com:YOUR-USERNAME/remote_conflicts_lab.git
```
Copy this URL — you'll need it in the next step.

> **Note on authentication:** You'll need your **SSH** key set up to follow these instructions.

**Step 2 — Clone it as "you" and add the starting files**

Open your terminal and run:

```bash
cd ~/Desktop
git clone git@github.com:YOUR-USERNAME/remote_conflicts_lab.git clone_you
cd clone_you
```

Create the starting files:
Create a new file `notes.txt`
```bash
nano notes.txt
```
Add the text below to `notes.txt`:
```
Project Notes
-------------
Meeting on Monday at 10am.
Bring your laptop.
Action items to follow.
```
Confirm contents of `notes.txt`
Create a new file `schedule.txt`
```bash
cat notes.txt
nano schedule.txt
```
Add the text below to `schedule.txt`:
```
Weekly Schedule
---------------
Monday: Team meeting
Tuesday: Design review
Wednesday: Free
Thursday: Client call
Friday: Wrap-up
```
Confirm contents of `schedule.txt`
Create a new file `ideas.txt`
```bash
cat schedule.txt
nano ideas.txt
```
Add the text below to `ideas.txt`:
```
Project Ideas
-------------
Idea 1: Redesign the homepage.
Idea 2: Add a contact form.
Idea 3: Improve mobile layout.
```
Confirm contents of `ideas.txt`
```bash
cat ideas.txt
git add .        # . adds everything (only recommended for a first commit)
git commit -m "Initial files: notes, schedule, ideas"
git push origin main
```
> **Checkpoint:** `git log --oneline` should show one commit. Go to GitHub website. View `remote_conflicts_lab` repository. You should see new files.

**Step 3 — Clone it again as your "colleague"**

```bash
cd ..                   # ensure you are in the parent directory of `clone_you` folder
git clone git@github.com:YOUR-USERNAME/remote_conflicts_lab.git clone_colleague
```

> **Checkpoint:** You should now have two folders side by side: `clone_you` and `clone_colleague`. `clone_colleague` should have the same three files that you created and pushed inside `clone_you`

```bash
ls clone_colleague/              # shows ideas.txt	notes.txt	schedule.txt
ls clone_you/            # shows ideas.txt	notes.txt	schedule.txt
```
---

## Conflict 1: Push rejected

**File:** `notes.txt`

Your colleague pushes a change while you are also working on the same file. When you try to push, Git rejects it because your history is behind theirs.

**Step 1 — Your colleague pushes first**

```bash
cd clone_colleague
nano notes.txt
```
Change the line `Meeting on Monday at 10am.` to `Meeting on Monday at 10am in the main conference room.`

Commit and push:
```bash
git diff notes.txt
git add notes.txt
git commit -m "Colleague: add room to Monday meeting"
git push origin main
```

**Step 2 — You make a different change and try to push**

```bash
cd ../clone_you
nano notes.txt
```
Add a new line that says `Bring snacks to share.`

Save and try to push:
```bash
git diff notes.txt
git add notes.txt
git commit -m "You: remind people to bring snacks"
git push origin main
```

You will see a rejection message like:

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:YOUR-USERNAME/remote_conflicts_lab.git'
...
```

> **Reading the message:** Git is saying: "Someone pushed since you last checked. Pull their changes first, then try again."

**Step 3 — Pull, resolve, then push**

```bash
git pull origin main
```
Depending on your git settings, you might see a rejection message like::
```bash
From github.com:YOUR-USERNAME/remote_conflicts_lab
 * branch            main       -> FETCH_HEAD
   7f80970..c4aa29f  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
...
```

Because you are working on the same branch you now have *divergent branches*. Git can try to reconcile the branches. The two most common options are: `merge` and `rebase`.

Let's ask Git to use `merge` by default
```bash
git config pull.rebase false      # merge
git pull origin main
```

Since the changes are on **different lines** Git will merge automatically and generate a new commit. 
You need to approve the commit message though! Save the changes and exit nano (`^o + ENTER + ^x`)

Check that both changes are in `notes.txt` and push:
```bash
cat notes.txt
git log --oneline --graph     # shows new merge commit
git push origin main          # push!
```

> **Success:** Your push is accepted. `notes.txt` includes both your change and your colleague's. `git log --oneline --graph` shows a merge commit.

> **Optional:** Go to GitHub website. View `remote_conflicts_lab` repository. Update the page to see the most recent commit message of `notes.txt`

---

## Conflict 2: Pull conflict

**File:** `schedule.txt`

This time, both you and your colleague edit the **same line** of the same file before either of you pulls. When you pull, Git can't merge automatically and stops to ask you to decide.

**Step 1 — Your colleague edits and pushes**

```bash
cd ../clone_colleague
git pull origin main
nano schedule.txt
```
Change the line `Wednesday: Free` to `Wednesday: Workshop (morning)`

Commit and push:
```bash
git diff schedule.txt
git add schedule.txt
git commit -m "Colleague: add workshop to Wednesday"
git push origin main
```

**Step 2 — You edit the same line without pulling first**

```bash
cd ../clone_you
nano schedule.txt
```
Change the line `Wednesday: Free` to `Wednesday: Office day`

Save and commit (but do not push yet):
```bash
git diff schedule.txt
git add schedule.txt
git commit -m "You: mark Wednesday as office day"
```

**Step 3 — Pull and see the conflict**

```bash
git pull origin main
```

Git stops and reports a conflict:

```
CONFLICT (content): Merge conflict in schedule.txt
...
```
Git tells us there is a conflict that needs to be solved manually.
```bash
nano schedule.txt
```
You'll see something like:
```
<<<<<<< HEAD
Wednesday: Office day
=======
Wednesday: Workshop (morning)
>>>>>>> origin/main
```

> **Reading the markers:**
> - `<<<<<<< HEAD` → your local version
> - `=======` → dividing line
> - `>>>>>>> origin/main` → the version from the remote

**Step 4 — Resolve and push**

Both are valid commitments. Write one line that combines them, for example:

```
Wednesday: Workshop (morning), office day after lunch
```
> **Nano tip:** Use `^K` to delete a whole line.

Delete all three marker lines and save. Then confirm the fix and finish:
```bash
git diff schedule.txt
git add schedule.txt
git commit -m "Resolve: combine Wednesday workshop and office day"
git push origin main
```

> **Success:** `schedule.txt` has no conflict markers. `git push` is accepted. `git log --oneline --graph` shows a merge commit.

---

## Conflict 3: Diverged history

**File:** `ideas.txt`

Both of you add a new idea to the end of the file while offline. When you pull, Git sees two separate histories that have "diverged" — neither is simply ahead of the clone_colleague.

**Step 1 — Your colleague adds an idea and pushes**

```bash
cd ../clone_colleague
git pull origin main
nano ideas.txt
```
Add a new line at the bottom: `Idea 4: Send a monthly newsletter.`

Save and push:
```bash
git diff ideas.txt
git add ideas.txt
git commit -m "Colleague: add newsletter idea"
git push origin main
```

**Step 2 — You add a different idea without pulling first**

```bash
cd ../clone_you
nano ideas.txt
```
Add a new line at the bottom: `Idea 4: Create a blog section.`

Save and commit:
```bash
git diff ideas.txt
git add ideas.txt
git commit -m "You: add blog idea"
```

**Step 3 — Pull and see the conflict**

```bash
git pull origin main
```

Git reports:
```
CONFLICT (content): Merge conflict in ideas.txt
Automatic merge failed; fix conflicts then commit the result.
```

Open the file:
```bash
nano ideas.txt
```

You'll see:
```
<<<<<<< HEAD
Idea 4: Create a blog section.
=======
Idea 4: Send a monthly newsletter.
>>>>>>> origin/main
```

Both ideas are good — the problem is only that they share the same line number. You want to **keep both**.

**Step 4 — Keep both, renumber, and push**

Edit the file so it reads:
```
Project Ideas
=============
Idea 1: Redesign the homepage.
Idea 2: Add a contact form.
Idea 3: Improve mobile layout.
Idea 4: Create a blog section.
Idea 5: Send a monthly newsletter.
```

Delete all conflict markers, save, then:
```bash
git diff ideas.txt
git add ideas.txt
git commit -m "Resolve: keep both new ideas, renumber to 4 and 5"
git push origin main
```

Verify the remote has all five ideas:
```bash
cd ../clone_colleague
git pull origin main
cat ideas.txt
```

> **Success:** Both clones now show five ideas, no conflict markers, and `git log --oneline --graph` in either clone shows the full shared history. You can also open your repository on GitHub and browse `ideas.txt` there to confirm the final version is on the remote.

---

## Bonus challenge

Practise pulling before you start work — the habit that prevents most remote conflicts.

```bash
cd ../clone_you

# Always start your day like this:
git pull origin main

# Now make a change, knowing you're up to date
nano notes.txt
```
Add a new line at the bottom: `Next review: end of month.`
```bash
git add notes.txt
git commit -m "Add next review note"
git push origin main
```

Check that your colleague can receive it cleanly:
```bash
cd ../clone_colleague
git pull origin main
cat notes.txt
```

> **The habit:** Pull before you start, pull before you push. The shorter the gap between your work and the remote, the smaller any conflict will be.

---


## Reflection questions

- In Conflict 1, why did Git reject your push instead of just merging the two versions automatically on the remote?
- In Conflict 2, what would have happened if you had pulled before editing `schedule.txt`?
- What is the difference between `git fetch` and `git pull`? 
- In what situation might `git fetch` be more useful?
- How does the habit of pulling before you start work reduce the chance of conflicts like these?
