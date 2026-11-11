# PRACTICAL | Conflicts with Remote Repositories (Pair Exercise)

You'll play the roles **Partner A** and **Partner One**. Decide now who is who — Partner A owns the repository on GitHub, Partner One is invited as a collaborator.

👀 Watch your partner work so that you can learn from each other.

## What is a remote conflict?

When you and a partner both change the same file and try to share your work through a shared remote repository, Git will stop and ask you to sort it out. This happens in two situations:

- **Push rejected** — you try to upload your work, but your partner pushed first. Git won't overwrite their changes.
- **Pull conflict** — you download your partner's changes, but they clash with edits you've already made locally.

Git will never silently overwrite work. A rejected push or a pull conflict is Git protecting your team's work — not a sign that something has broken.

---

## Setup

### Step 1 — Partner A checks the `free_fall` repository

**Partner A:**

- Navigate to your `free_fall` local repository
- Check the status of git. You should be on `main` with no local changes
- Check that `origin` points to **your own** GitHub repository
- If `main` has commits that are not on GitHub yet, push them so that GitHub is up to date

<details>
<summary>🔍 Click here for hints! </summary>

- To see the status use `git status`. It should say `Your branch is up to date with 'origin/main'`
- To see the remote repositories use `git remote -v`
- To push to remote use `git push origin main`
</details>

### Step 2 — Partner A adds Partner One as a collaborator

**Partner A:**

- On GitHub, open your `free_fall` repository and go to **Settings → Collaborators**
- Click **Add people**
- Enter Partner One's GitHub username or the email tied to their GitHub account
- Send the invite

**Partner One:**

- Check your email or your GitHub notifications for the invite
- Accept it

🔔 **Partner One confirms to Partner A** that the invite has been accepted

### Step 3 — Partner A adds the starting file

**Partner A**, in your `free_fall` folder:

- Create `velocity_height.py` using nano and add the text below:
  ```python
  # Final velocity of an object dropped from a given height
  gravity = 9.81
  height = 20
  import math
  velocity = math.sqrt(2 * gravity * height)
  print("An object dropped from", height, "m hits the ground at", velocity, "m/s")
  ```
- Run `python velocity_height.py`. You should see `An object dropped from 20 m hits the ground at 19.809088823063014 m/s`
- Stage `velocity_height.py` and commit with the message `Add velocity_height.py`
- Push to the remote repository


<details>
<summary>🔍 Click here for hints! </summary>

- To create a new file with nano use `nano name_of_file`
- To stage a file use `git add name_of_file`
- To commit use `git commit -m "commit message"`
- To push to remote use `git push origin main`
</details>

> **Checkpoint:** On GitHub, you should see `velocity_height.py` in the repository `free_fall`, and Partner One listed under Settings → Collaborators

🔔 **Tell Partner One the new file is pushed and they can clone**

### Step 4 — Partner One clones the same repository

You already have your own `free_fall` folder. To avoid mixing the two, you will clone Partner A's repository into a **different folder name**.

**Partner One**, open your terminal and:
- Move into your `Desktop` directory
- Clone the repository at `git@github.com:PARTNER-A-USERNAME/free_fall.git` into a folder named `free_fall_partner`
- Move into the new `free_fall_partner` folder
- List the folder's contents. You should see `velocity_height.py` together with the other `.py` files

<details>
<summary>🔍 Click here for hints! </summary>

- To clone into a specific folder use `git clone url_of_repo name_of_folder`
</details>

> **Checkpoint:** Each partner now has their own copy of the repository, on their own machine, both containing `velocity_height.py`. Partner One: **do not touch your own `free_fall` folder** during this practical — you work in `free_fall_partner`

---

## 💪 Conflict 1: Push rejected

Partner One improves the comment at the top of `velocity_height.py`. Partner A adds a conversion to km/h at the bottom. They edit **different lines**, but Partner A will still be rejected when pushing.

**Step 1 — Partner One pushes first**

**Partner One**, in your `free_fall_partner` folder:
- Open `velocity_height.py` for editing
- Modify the first line (the comment) so that it shows the formula. The line should now read:
```python
# Final velocity of an object dropped from a given height: v = sqrt(2 * g * h)
```
- Check the differences in `velocity_height.py`
- Stage `velocity_height.py` and commit with the message `One: add formula to comment`
- Push changes to remote

<details>
<summary>🔍 Click here for hints! </summary>

- To see the changes in a file use `git diff name_of_file`
- To stage a file use `git add name_of_file`
- To commit use `git commit -m "commit message"`
- To push to remote use `git push origin main`
</details>

🔔 **Tell Partner A you've pushed.**

**Step 2 — Partner A makes a different change and tries to push**

**Partner A**, in your `free_fall` folder (do this only after Partner One tells you they've pushed):
- Open `velocity_height.py` for editing
- Add two new lines at the bottom and save:
```python
velocity_kmh = velocity * 3.6
print("or", velocity_kmh, "km/h")
```
- Check the differences in `velocity_height.py`
- Stage `velocity_height.py` and commit with the message `A: add velocity in km/h`
- Push changes to remote

You will see a rejection message like:

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:PARTNER-A-USERNAME/free_fall.git'
...
```
<details>
<summary>🔍 Click here for hints! </summary>

- To see the changes in a file use `git diff name_of_file`
- To stage a file use `git add name_of_file`
- To commit use `git commit -m "commit message"`
- To push to remote use `git push origin main`
</details>

> **Reading the message:** Git is saying: "Someone pushed since you last checked. Pull their changes first, then try again."

**Step 3 — Partner A pulls, resolves, then pushes**

**Partner A:**
- Pull the latest changes from `origin/main`

Depending on Partner A's git settings, there might be a message like:
```
From github.com:PARTNER-A-USERNAME/free_fall
 * branch            main       -> FETCH_HEAD
   7f80970..c4aa29f  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
...
```

Ask Git to reconcile the branches using `merge` by default:
- Set your pull strategy to merge (`pull.rebase` set to `false`)
- Try to pull again

Since the changes are on **different lines**, Git will merge automatically and generate a new commit. You need to approve the commit message though! Save the changes and exit nano (`^o + ENTER + ^x`).

Check that both changes are in `velocity_height.py` and push:
- View the contents of `velocity_height.py`
- Run `python velocity_height.py`. You should see two lines: `An object dropped from 20 m hits the ground at 19.809088823063014 m/s` and `or 71.31271976302685 km/h`
- View the commit graph. It should look like this:

```bash
*   xxxxxxx (HEAD -> main) Merge branch 'main' of github.com:...
|\  
| * xxxxxxx (origin/main) One: add formula to comment
* | xxxxxxx A: add velocity in km/h
|/  
* xxxxxxx Add velocity_height.py
```
- Push changes to remote

<details>
<summary>🔍 Click here for hints! </summary>

- To pull from remote use `git pull origin main`
- To set your pull strategy to merge use `git config pull.rebase false`
- To see the commit graph use `git log --oneline --graph` or the alias `git lg`
- To push to remote use `git push origin main`
</details>


🔔 **Tell Partner One you've pushed the merge.**

**Partner One**, sync up so you're both on the same page:
- Pull the latest changes from `origin/main`
- View the contents of `velocity_height.py`
- View the commit graph

> **Success:** For both partners, `velocity_height.py` includes both changes (the new comment and the km/h lines) and the commit graph shows a merge commit.

---

## 💪 Conflict 2: Pull conflict

Both partners improve the wording of the **same** `print` line in `velocity_height.py`. Their edits are different, but both are good, so we will combine them.

**Step 1 — Partner A edits and pushes**

**Partner A:**
- Pull the latest changes from `origin/main`
- Open `velocity_height.py` for editing
- Modify the `print` line that starts with `print("An object dropped from"` so that `An object dropped from` becomes `A stone dropped from`. The line should now read:
```python
print("A stone dropped from", height, "m hits the ground at", velocity, "m/s")
```
- Check the differences in `velocity_height.py`
- Stage `velocity_height.py` and commit with the message `A: say stone instead of object`
- Push changes to remote

🔔 **Partner One - DO NOT PULL YET** — we are simulating a case where Partner One edits the same line without "knowing" there are changes on the remote.

**Step 2 — Partner One edits the same line without pulling first**

**Partner One:**
- Open `velocity_height.py` for editing
- Modify the **same** `print` line so that `hits the ground at` becomes `hits the Earth's surface at`. The line should now read:
```python
print("An object dropped from", height, "m hits the Earth's surface at", velocity, "m/s")
```
- Check the differences in `velocity_height.py`
- Stage `velocity_height.py` and commit with the message `One: say Earth surface`


**Step 3 — Partner One pulls and sees the conflict**

**Partner One:**
- Pull the latest changes from `origin/main`

Depending on Partner One's git settings, there might be a message like:
```
From github.com:PARTNER-A-USERNAME/free_fall
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
CONFLICT (content): Merge conflict in velocity_height.py
...
```

- Check the status of git. `velocity_height.py` should be listed as `both modified`
- Open `velocity_height.py` with `nano` — you'll see:

```
<<<<<<< HEAD
print("An object dropped from", height, "m hits the Earth's surface at", velocity, "m/s")
=======
print("A stone dropped from", height, "m hits the ground at", velocity, "m/s")
>>>>>>> <HASH>
```

> **Reading the markers:**
> - `<<<<<<< HEAD` → your local version
> - `=======` → dividing line
> - `>>>>>>> <HASH>` → the version from the remote

**Step 4 — Resolve and push**

Both of your changes are valid. **Discuss with your partner** and write one line that combines them:
```python
print("A stone dropped from", height, "m hits the Earth's surface at", velocity, "m/s")
```
> **Nano tip:** Use `^K` to delete a whole line.

**Partner One** deletes all three marker lines (and the versions of the `print` line you don't need), saves, then:
- Use `cat` to confirm the contents of `velocity_height.py`
- Run `python velocity_height.py`. You should see `A stone dropped from 20 m hits the Earth's surface at 19.809088823063014 m/s` followed by the km/h line
- Check the differences in `velocity_height.py`
- Stage `velocity_height.py` and commit with the message `Resolve: combine stone and Earth surface`
- Push changes to remote

<details>
<summary>🔍 Click here for hints! </summary>

- To pull from remote use `git pull origin main`
- To set your pull strategy to merge use `git config pull.rebase false`
- To see the status use `git status`
- To push to remote use `git push origin main`
</details>

🔔 **Tell Partner A to pull.**

**Partner A:**
- Pull the latest changes from `origin/main`
- View the contents of `velocity_height.py`
- View the commit graph

> **Success:** `velocity_height.py` has no conflict markers on either machine. `git push` was accepted. The commit graph shows a new merge commit on `main`.

---

## 🚀 Optional challenge: Diverged history (branches + Pull Request on GitHub)

This time you'll each work on your **own branch** and merge using a **Pull Request (PR)** in GitHub. A PR is a tool to merge the changes from a `branch` into `main`.

**Step 1 — Partner One branches, adds a conversion, opens a PR**

**Partner One:**
- Pull the latest changes from `origin/main`
- Create and switch to a new branch called `mph`
- Open `velocity_height.py` for editing
- Add two new lines at the bottom:
```python
velocity_mph = velocity * 2.237
print("or", velocity_mph, "mph")
```
- Check the differences in `velocity_height.py`
- Stage `velocity_height.py` and commit with the message `One: add velocity in mph`
- Push the branch to remote

<details>
<summary>🔍 Click here for hints! </summary>

- Create and switch to a branch: `git switch -c branch-name`
- Push a new branch: `git push origin branch-name`
</details>

- Go to GitHub
- Since a new branch was pushed, GitHub automatically offers to `Compare & pull request` 
- Click on the `Compare & pull request` icon
- A `Comparing changes` window will show. Browse down to see the changes at the bottom. Only your two new lines should appear
- Add a mini description like `Add mph` and click on `Create pull request`
- **Do not merge it yet.**

🔔 **Tell Partner A you've opened a PR — don't merge yet.**

**Step 2 — Partner A branches, adds a different line, opens a PR**

**Partner A:**
- Pull the latest changes from `origin/main`
- Create and switch to a new branch called `earth_note`
- Open `velocity_height.py` for editing
- Add one new line at the bottom:
```python
print("under Earth gravity of", gravity, "m/s2")
```
- Check the differences in `velocity_height.py`
- Stage `velocity_height.py` and commit with the message `A: mention Earth gravity`
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
- Add a mini description like `Add Earth gravity note` and click on `Create pull request`
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

Both additions are good — the problem is only that both were added at the same place, at the end of the file. Edit the web editor so the file reads:
```python
# Final velocity of an object dropped from a given height: v = sqrt(2 * g * h)
gravity = 9.81
height = 20
import math
velocity = math.sqrt(2 * gravity * height)
print("A stone dropped from", height, "m hits the Earth's surface at", velocity, "m/s")
velocity_kmh = velocity * 3.6
print("or", velocity_kmh, "km/h")
velocity_mph = velocity * 2.237
print("or", velocity_mph, "mph")
print("under Earth gravity of", gravity, "m/s2")
```
- Delete all conflict markers
- Click **Mark as resolved**, then **Commit merge**
- Merge the Pull Request. GitHub might need a moment to enable the merge. Patience 🧘

🔔 **Tell Partner One the PR is merged — time to pull.**

**Step 6 — Both partners sync locally and clean up**

**Partner A and Partner One:**
- Switch back to your local `main` branch
- Pull the latest changes from `origin/main`
- View the contents of `velocity_height.py`
- Run `python velocity_height.py`. You should see four lines, the last one being `under Earth gravity of 9.81 m/s2`
- Now that your branch is merged, delete your local branch (`mph` for Partner One, `earth_note` for Partner A)
- View the commit graph for all branches

<details>
<summary>🔍 Click here for hints! </summary>

- To switch between branches use `git switch name_of_branch`
- To pull from remote use `git pull origin main`
- To delete a branch use `git branch -d name_of_branch`
</details>

> **Success:** Both of you now see the km/h lines, the mph lines and the Earth gravity note in `velocity_height.py`, no conflict markers, and both PRs appear as **Merged** on GitHub. The commit graph shows new merge commits on top of `main`.

---

## Reflection questions

- In Conflict 1, Partner A edited a different line than Partner One. Why was the push still rejected?
- What is the difference between a **push rejected** and a **pull conflict**? At which moment does Git detect each of them?
- Why does Git ask you to pull before you push, rather than just overwriting the remote?
- Why is it useful to work on separate branches and merge with a PR instead of everyone pushing straight to `main`?
- What habits would help you and your collaborators run into fewer conflicts in a real project?
