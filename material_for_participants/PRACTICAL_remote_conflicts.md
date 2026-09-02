# PRACTICAL | Conflicts with Remote Repositories (Pair Exercise)

This version is done with a real partner instead of two clones on one machine. You'll play the roles **Partner A** and **Partner B**. Decide now who is who — Partner A owns the repository, Partner B is invited as a collaborator.

At several points you must **wait for your partner** before continuing — these are marked with 🔔.

---

## What is a remote conflict?

When you and a partner both change the same file and try to share your work through a shared remote repository, Git will stop and ask you to sort it out. This happens in two situations:

- **Push rejected** — you try to upload your work, but your partner pushed first. Git won't overwrite their changes.
- **Pull conflict** — you download your partner's changes, but they clash with edits you've already made locally.

Git will never silently overwrite work. A rejected push or a pull conflict is Git protecting your team's work — not a sign that something has broken.

---

## Escape hatches

| Command | What it does |
|---|---|
| `git merge --abort` | Cancel a merge in progress — safe at any time |
| `git pull origin main` | Fetch and merge the remote's latest changes |
| `git push origin main` | Upload your commits to the remote |
| `git fetch origin` | Download remote changes without merging yet |

---

## Setup

> **Note on authentication:** You'll need your **SSH** key set up to follow these instructions.

### Step 1 — Partner A creates the repository on GitHub

**Partner A:**

- Go to [github.com](https://github.com) and sign in
- Click the **+** icon in the top-right corner and choose **New repository**
- Name it `remote_conflicts`
- Leave it **Public**
- Leave all other options as they are. Do **not** add a README or .gitignore
- Click **Create repository**

Click on the `SSH` tab and copy the clone URL. It will look like:
```
git@github.com:PARTNER-A-USERNAME/remote_conflicts.git
```

### Step 2 — Partner A adds Partner B as a collaborator

**Partner A:**

- In the new repository, go to **Settings → Collaborators**
- Click **Add people**
- Enter Partner B's GitHub username or the email tied to their GitHub account
- Send the invite

🔔 **Tell Partner B their GitHub invite has been sent**

**Partner B:**

- Check your email or your GitHub notifications for the invite
- Accept it

🔔 **Partner B confirms to Partner A** that the invite has been accepted

### Step 3 — Partner A clones it and adds the starting files

**Partner A**, open your terminal and run:

```bash
cd ~/Desktop
git clone git@github.com:PARTNER-A-USERNAME/remote_conflicts.git remote_conflicts
cd remote_conflicts
```

You are **encouraged to copy/paste** the contents of the files for this section.

Create a new file `notes.txt`:
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
Confirm contents of `notes.txt`, then create `schedule.txt`:
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
Confirm contents of `schedule.txt`, then create `ideas.txt`:
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
Confirm contents of `ideas.txt`:
```bash
cat ideas.txt
git add .        # . adds everything (only recommended for a first commit)
git commit -m "Initial files: notes, schedule, ideas"
git push origin main
```

> **Checkpoint:** `git log --oneline` should show one commit. Go to GitHub and view the `remote_conflicts` repository. You should see the three new files, and Partner B listed under Settings → Collaborators

🔔 **Tell Partner B the initial files are pushed and they can clone**

### Step 4 — Partner B clones the same repository

From here on, **avoid copy-pasting**. Typing all the commands helps you build understanding.

**Partner B**, open your terminal and run:

```bash
cd ~/Desktop
git clone git@github.com:PARTNER-A-USERNAME/remote_conflicts.git remote_conflicts
cd remote_conflicts
```

> **Checkpoint:** Each partner now has their own `remote_conflicts` folder, on their own machine, cloned from the same GitHub repo

```bash
ls    # both partners should see: ideas.txt  notes.txt  schedule.txt
```

---

## Conflict 1: Push rejected

> **Situation:** File `notes.txt`. Partner B pushes a change while Partner A is also working on the same file. When Partner A tries to push, Git rejects it because their history is behind.

**Step 1 — Partner B pushes first**

**Partner B**, in your `remote_conflicts` folder:
```bash
nano notes.txt
```
Change the line `Meeting on Monday at 10am.` to `Meeting on Monday at 10am in the main conference room.`

Commit and push:
```bash
git diff notes.txt
git add notes.txt
git commit -m "Partner B: add room to Monday meeting"
git push origin main
```

🔔 **Tell Partner A you've pushed.**

**Step 2 — Partner A makes a different change and tries to push**

**Partner A**, in your `remote_conflicts` folder (do this only after Partner B tells you they've pushed):
```bash
nano notes.txt
```
Add a new line that says `Bring snacks to share.`

Save and try to push:
```bash
git diff notes.txt
git add notes.txt
git commit -m "Partner A: remind people to bring snacks"
git push origin main
```

You will see a rejection message like:

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:PARTNER-A-USERNAME/remote_conflicts.git'
...
```

> **Reading the message:** Git is saying: "Someone pushed since you last checked. Pull their changes first, then try again."

**Step 3 — Partner A pulls, resolves, then pushes**

**Partner A:**
```bash
git pull origin main
```
Depending on your git settings, you might see a message like:
```bash
From github.com:PARTNER-A-USERNAME/remote_conflicts
 * branch            main       -> FETCH_HEAD
   7f80970..c4aa29f  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
...
```

Because you are working on the same branch you now have *divergent branches*. Git can try to reconcile them. The two most common options are `merge` and `rebase`.

Ask Git to use `merge` by default:
```bash
git config pull.rebase false      # merge
git pull origin main
```

Since the changes are on **different lines**, Git will merge automatically and generate a new commit. You need to approve the commit message though! Save the changes and exit nano (`^o + ENTER + ^x`).

Check that both changes are in `notes.txt` and push:
```bash
cat notes.txt
git log --oneline --graph     # shows new merge commit
git push origin main          # push!
```

> **Success:** Partner A's push is accepted. `notes.txt` includes both changes. `git log --oneline --graph` shows a merge commit.

🔔 **Tell Partner B you've pushed the merge.**

**Partner B**, sync up so you're both on the same page:
```bash
git pull origin main
cat notes.txt
```

---

## Conflict 2: Pull conflict

> **Situation:** File `schedule.txt`. This time, both partners edit the **same line** before either of you pulls. When you pull, Git can't merge automatically and stops to ask you to decide.

**Step 1 — Partner B edits and pushes**

**Partner B:**
```bash
git pull origin main
nano schedule.txt
```
Change the line `Wednesday: Free` to `Wednesday: Workshop (morning)`

Commit and push:
```bash
git diff schedule.txt
git add schedule.txt
git commit -m "Partner B: add workshop to Wednesday"
git push origin main
```

🔔 **Don't tell Partner A yet** — the point of this exercise is that Partner A edits the same line without knowing.

**Step 2 — Partner A edits the same line without pulling first**

**Partner A:**
```bash
nano schedule.txt
```
Change the line `Wednesday: Free` to `Wednesday: Office day`

Save and commit (but do not push yet):
```bash
git diff schedule.txt
git add schedule.txt
git commit -m "Partner A: mark Wednesday as office day"
```

🔔 **Now tell each other what you each did**, then Partner A continues.

**Step 3 — Partner A pulls and sees the conflict**

**Partner A:**
```bash
git pull origin main
```

Git stops and reports a conflict:
```
CONFLICT (content): Merge conflict in schedule.txt
...
```

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

Both of your changes are valid. **Discuss with your partner** and write one line that combines them, for example:
```
Wednesday: Workshop (morning), office day after lunch
```
> **Nano tip:** Use `^K` to delete a whole line.

**Partner A** deletes all three marker lines, saves, then:
```bash
git diff schedule.txt
git add schedule.txt
git commit -m "Resolve: combine Wednesday workshop and office day"
git push origin main
```

> **Success:** `schedule.txt` has no conflict markers. `git push` is accepted. `git log --oneline --graph` shows a merge commit.

🔔 **Tell Partner B to pull.**

**Partner B:**
```bash
git pull origin main
cat schedule.txt
```

---

## Conflict 3: Diverged history

> **Situation:** File `ideas.txt`. Both of you add a new idea to the end of the file while "offline" (i.e. without checking in with each other or pulling). When you pull, Git sees two separate histories that have "diverged" — neither partner is simply ahead of the other.

**Step 1 — Partner B adds an idea and pushes**

**Partner B:**
```bash
git pull origin main
nano ideas.txt
```
Add a new line at the bottom: `Idea 4: Send a monthly newsletter.`

Save and push:
```bash
git diff ideas.txt
git add ideas.txt
git commit -m "Partner B: add newsletter idea"
git push origin main
```

🔔 **Again, hold off telling Partner A** until after their commit in Step 2.

**Step 2 — Partner A adds a different idea without pulling first**

**Partner A:**
```bash
nano ideas.txt
```
Add a new line at the bottom: `Idea 4: Create a blog section.`

Save and commit:
```bash
git diff ideas.txt
git add ideas.txt
git commit -m "Partner A: add blog idea"
```

🔔 **Compare notes with your partner**, then Partner A continues.

**Step 3 — Partner A pulls and sees the conflict**

**Partner A:**
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

**Partner A**, edit the file so it reads:
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

🔔 **Tell Partner B to pull and verify.**

**Partner B:**
```bash
git pull origin main
cat ideas.txt
```

> **Success:** Both of you now show five ideas, no conflict markers, and `git log --oneline --graph` shows the full shared history on both machines. You can also open the repository on GitHub and browse `ideas.txt` there to confirm the final version is on the remote.

---

## Bonus challenge — swap roles

Practise pulling before you start work — the habit that prevents most remote conflicts. This time, **swap roles**: Partner B goes first.

**Partner B:**
```bash
# Always start your day like this:
git pull origin main
```
Now make a change, knowing you're up to date:
```bash
nano notes.txt
```
Add a new line at the bottom: `Next review: end of month.`
```bash
git add notes.txt
git commit -m "Add next review note"
git push origin main
```

🔔 **Tell Partner A to pull.**

**Partner A**, check that you can receive it cleanly:
```bash
git pull origin main
cat notes.txt
```

> **The habit:** Pull before you start, pull before you push. The shorter the gap between your work and your partner's, the smaller any conflict will be — and the less you need the 🔔 check-ins this exercise forced on you.



