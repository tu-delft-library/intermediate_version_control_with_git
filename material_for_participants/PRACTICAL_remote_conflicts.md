# PRACTICAL | Conflicts with Remote Repositories (Pair Exercise)

You'll play the roles **Partner A** and **Partner One**. Decide now who is who — Partner A owns the repository, Partner One is invited as a collaborator.

👀 Watch your partner work so that you can learn from each other.


---

## What is a remote conflict?

When you and a partner both change the same file and try to share your work through a shared remote repository, Git will stop and ask you to sort it out. This happens in two situations:

- **Push rejected** — you try to upload your work, but your partner pushed first. Git won't overwrite their changes.
- **Pull conflict** — you download your partner's changes, but they clash with edits you've already made locally.

Git will never silently overwrite work. A rejected push or a pull conflict is Git protecting your team's work — not a sign that something has broken.

---

## Setup

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

### Step 2 — Partner A adds Partner One as a collaborator

**Partner A:**

- In the new repository, go to **Settings → Collaborators**
- Click **Add people**
- Enter Partner One's GitHub username or the email tied to their GitHub account
- Send the invite

**Partner One:**

- Check your email or your GitHub notifications for the invite
- Accept it

🔔 **Partner One confirms to Partner A** that the invite has been accepted

### Step 3 — Partner A clones it and adds the starting files

**Partner A**, open your terminal and run:

```bash
cd ~/Desktop
git clone git@github.com:PARTNER-A-USERNAME/remote_conflicts.git remote_conflicts
cd remote_conflicts
```

You are **encouraged to copy/paste** the contents of the files for this section.

- Create `notes.txt` using nano and add the text below:
  ```
  Project Notes
  -------------
  Meeting on Monday at 10am.
  Bring your laptop.
  Action items to follow.
  ```
- Create `schedule.txt` using nano and add the text below:
  ```
  Weekly Schedule
  ---------------
  Monday: Team meeting
  Tuesday: Design review
  Wednesday: Free
  Thursday: Client call
  Friday: Wrap-up
  ```
- Create `ideas.txt` using nano and add the text below:
  ```
  Project Ideas
  -------------
  Idea 1: Redesign the homepage.
  Idea 2: Add a contact form.
  Idea 3: Improve mobile layout.
  ```
- Stage and commit all files together with the message `Initial files`
- Push to the remote repository


<details>
<summary>🔍 Click here for hints! </summary>

- To create a new file with nano use `nano name_of_file`
- To stage files use `git add name_of_file another_file`
- To commit `git commit -m "commit message"`
- To push to remote use `git push origin main`
</details>

> **Checkpoint:** You should see the three new files in the GitHub repository `remote_conflicts`, and Partner One listed under Settings → Collaborators

🔔 **Tell Partner One the initial files are pushed and they can clone**

### Step 4 — Partner One clones the same repository

From here on, **avoid copy-pasting**. Typing all the commands helps you build understanding.

**Partner One**, open your terminal and:
- Move into your `Desktop` directory
- Clone the repository at `git@github.com:PARTNER-A-USERNAME/remote_conflicts.git` into a folder named `remote_conflicts`
- Move into the new `remote_conflicts` folder

> **Checkpoint:** Each partner now has their own `remote_conflicts` folder, on their own machine, cloned from the same GitHub repo

- List the folder's contents — both partners should see: `ideas.txt`, `notes.txt`, `schedule.txt`

---

## 💪 Conflict 1: Push rejected

**Step 1 — Partner One pushes first**

**Partner One**, in your `remote_conflicts` folder:
- Open `notes.txt` for editing
- Change the line `Meeting on Monday at 10am.` to `Meeting on Monday at 10am in the Orange room.`
- Check the differences in `notes.txt`
- Stage `notes.txt` and commit with the message `One: add room to Monday meeting`
- Push changes to remote

<details>
<summary>🔍 Click here for hints! </summary>

- To see the changes in a file use `git diff name_of_file`
- To stage files use `git add name_of_file another_file`
- To commit `git commit -m "commit message"`
- To push to remote use `git push origin main`
</details>

🔔 **Tell Partner A you've pushed.**

**Step 2 — Partner A makes a different change and tries to push**

**Partner A**, in your `remote_conflicts` folder (do this only after Partner One tells you they've pushed):
- Open `notes.txt` for editing
- Add a new line that says `Bring snacks.` and save
- Check the differences in `notes.txt`
- Stage `notes.txt` and commit with the message `A: reminder to bring snacks`
- Push changes to remote

You will see a rejection message like:

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:PARTNER-A-USERNAME/remote_conflicts.git'
...
```
<details>
<summary>🔍 Click here for hints! </summary>

- To see the changes in a file use `git diff name_of_file`
- To stage files use `git add name_of_file another_file`
- To commit `git commit -m "commit message"`
- To push to remote use `git push origin main`
</details>

> **Reading the message:** Git is saying: "Someone pushed since you last checked. Pull their changes first, then try again."

**Step 3 — Partner A pulls, resolves, then pushes**

**Partner A:**
- Pull the latest changes from `origin/main`

Depending on Partner A's git settings, there might be a message like:
```
From github.com:PARTNER-A-USERNAME/remote_conflicts
 * branch            main       -> FETCH_HEAD
   7f80970..c4aa29f  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
...
```

Ask Git to reconcile the branches using `merge` by default:
- Set your pull strategy to merge (`pull.rebase` set to `false`)
- Try to pull again

Since the changes are on **different lines**, Git will merge automatically and generate a new commit. You need to approve the commit message though! Save the changes and exit nano (`^o + ENTER + ^x`).

Check that both changes are in `notes.txt` and push:
- View the contents of `notes.txt`
- View the commit history as a graph
- Push changes to remote

<details>
<summary>🔍 Click here for hints! </summary>

- To pull from remote use `git pull origin main`
- To set your pull strategy to merge use `git config pull.rebase false`
</details>


🔔 **Tell Partner One you've pushed the merge.**

**Partner One**, sync up so you're both on the same page:
- Pull the latest changes from `origin/main`
- View the contents of `notes.txt`
- View the commit history as a graph

> **Success:** For both partner, `notes.txt` includes both changes and `git log --oneline --graph` shows a merge commit. 

---

## 💪 Conflict 2: Pull conflict


**Step 1 — Partner A edits and pushes**

**Partner A:**
- Pull the latest changes from `origin/main`
- Open `schedule.txt` for editing
- Change the line `Wednesday: Free` to `Wednesday: Workshop (morning)`
- Check the differences in `schedule.txt`
- Stage `schedule.txt` and commit with the message `A: add workshop to Wednesday`
- Push changes to remote

🔔 **Partner One - DO NOT PULL YET** — we are simulating a case where Partner One edits the same line without "knowing" there are changes on the remote.

**Step 2 — Partner One edits the same line without pulling first**

**Partner One:**
- Open `schedule.txt` for editing
- Change the line `Wednesday: Free` to `Wednesday: Office day`
- Check the differences in `schedule.txt`
- Stage `schedule.txt` and commit with the message `One: mark Wednesday as office day`


**Step 3 — Partner One pulls and sees the conflict**

**Partner One:**
- Pull the latest changes from `origin/main`

Depending on Partner One's git settings, there might be a message like:
```
From github.com:PARTNER-A-USERNAME/remote_conflicts
 * branch            main       -> FETCH_HEAD
   7f80970..c4aa29f  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
...
```
Ask Git to reconcile the branches using `merge` by default:
- Set your pull strategy to merge (`pull.rebase` set to `false`)
- Try to pull again

Git stops as it finds a conflict. Since the changes are on **the same line**, Git asks you to manually solve the conflict. 
```
CONFLICT (content): Merge conflict in schedule.txt
...
```

- Open `schedule.txt` for editing

> **Reading the markers:**
> - `<<<<<<< HEAD` → your local version
> - `=======` → dividing line
> - `>>>>>>> <HASH>` → the version from the remote

**Step 4 — Resolve and push**

Both of your changes are valid. **Discuss with your partner** and write one line that combines them, for example:
```
Wednesday: Workshop (morning), office day after lunch
```
> **Nano tip:** Use `^K` to delete a whole line.

**Partner One** deletes all three marker lines, saves, then:
- Check the differences in `schedule.txt`
- Stage `schedule.txt` and commit with the message `Resolve: combine Wednesday workshop and office day`
- Push changes to remote

🔔 **Tell Partner A to pull.**

**Partner A:**
- Pull the latest changes from `origin/main`
- View the contents of `schedule.txt`
- View the commit history as a graph

> **Success:** `schedule.txt` has no conflict markers. `git push` is accepted. `git log --oneline --graph` shows a merge commit.
---

## 🚀 Optional challenge: Diverged history (branches + Pull Request on GitHub)

This time you'll each work on a **branch** and merge using a **Pull Request (PR)** in GitHub. PR are a tool to merge the changes from `branches` into `main`. 

**Step 1 — Partner One branches, adds an idea, opens a PR**

**Partner One:**
- Pull the latest changes from `origin/main`
- Create and switch to a new branch called `newsletter`
- Open `ideas.txt` for editing
- Add a new line at the bottom: `Idea 4: Send a monthly newsletter.`
- Check the differences in `ideas.txt`
- Stage `ideas.txt` and commit with the message `One: add newsletter idea`
- Push the branch to remote

<details>
<summary>🔍 Click here for hints! </summary>

- Create and switch to a branch: `git switch -c branch-name`
- Push a new branch: `git push origin branch-name`
</details>

- Go to GitHub
- Since a new branch was pushed, GitHub automatically offers to `Compare & pull request` 
- Click on the `Compare & pull request` icon
- A `Comparing changes` window will show. Browse down to see the changes at the bottom
- Add a mini description like `Add newsletter idea` and click on `Create pull request`
- **Do not merge it yet.**

🔔 **Tell Partner A you've opened a PR — don't merge yet.**

**Step 2 — Partner A branches, adds a different idea, opens a PR**

**Partner A:**
- Pull the latest changes from `origin/main`
- Create and switch to a new branch called `blog`
- Open `ideas.txt` for editing
- Add a new line at the bottom: `Idea 4: Create a blog section.`
- Check the differences in `ideas.txt`
- Stage `ideas.txt` and commit with the message `A: add blog idea`
- Push the branch to remote

<details>
<summary>🔍 Click here for hints! </summary>

- Create and switch to a branch: `git switch -c branch-name`
- Push a new branch: `git push origin branch-name`
</details>


- Go to GitHub
- Since a new branch was pushed, GitHub automatically offers to `Compare & pull request` 
- Click on the `Compare & pull request` icon
- A `Comparing changes` window will show. Browse down to see the changes at the bottom
- Add a mini description like `Add blog idea` and click on `Create pull request`
- **Do not merge it yet.**

> Both PRs target `main`. Neither branch has been merged yet, so GitHub will happily let you open both.

**Step 3 — Merge the first PR (clean)**

**Whoever opened first (Partner One)** merges their PR on GitHub. Since `main` hasn't changed, this merges cleanly.

🔔 **Tell your partner the first PR is merged.**

**Step 4 — See the conflict on the second PR**

**Partner A**, go back to your open PR on GitHub. It now shows:
```
This branch has conflicts that must be resolved
```
because `main` moved on (Partner One's merge) since you branched.

- Click **Resolve conflicts** in the GitHub interface
- You'll see the same `<<<<<<<` / `=======` / `>>>>>>>` markers, but editable in the browser

**Step 5 — Resolve in the GitHub editor and merge**

Both ideas are good — the problem is only that they share the same line number. Edit the web editor so the file reads:
```
Project Ideas
=============
Idea 1: Redesign the homepage.
Idea 2: Add a contact form.
Idea 3: Improve mobile layout.
Idea 4: Create a blog section.
Idea 5: Send a monthly newsletter.
```
- Delete all conflict markers
- Click **Mark as resolved**, then **Commit merge**
- Merge the Pull Request

🔔 **Tell Partner One the PR is merged — time to pull.**

**Step 6 — Both partners sync locally**

**Partner A and Partner One:**
- Switch back to your local `main` branch
- Pull the latest changes from `origin/main`
- View the contents of `ideas.txt`


> **Success:** Both of you now show five ideas, no conflict markers, and both PRs appear as **Merged** on GitHub. `git log --oneline --graph` shows two merge commits on `main`.

---