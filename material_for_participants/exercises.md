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
<summary>🔍 Click here for a hint! </summary>

- To create use `git branch name_of_branch`
- To create and switch in one step, add the flag `-c` to `git switch`
- To rename use `git branch -m current_name_of_branch new_name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To check the history of a branch use `git log --oneline`
- To delete branches use `git branch -d name_of_branch name_of_another_branch`
</details>

#### 🚀 Optional challenge — safe delete vs force delete

1. Create a branch called `experiment` and switch to it in ONE STEP
1. Add a new line to `lines.txt`
1. Commit your changes
1. Switch back to main
1. Try to delete `experiment` with `git branch -d experiment`
1. Git will refuse. Read the error message carefully — why is it blocking you?
1. Follow the hint in the error message to force delete the branch


<details>
<summary>🔍 Explanation! </summary>

- `-d` is a safe delete — Git refuses if the branch has commits that have not been merged into the current branch. It is protecting you from losing work.
- `-D` is a force delete — Git deletes it regardless. The commits are not immediately gone but they are hard to recover.

>**As a rule of thumb:** if `-d` refuses, pause and ask yourself whether those commits are actually needed before reaching for `-D`.
</details>


## 2 💪 Commit on a secondary branch

Follow the steps listed below:

1. Move to branch `b2` (the file `lines.txt` contains three lines)
1. Append a **fourth** line using `echo`
1. Append a **fifth** line using `echo`
1. Make a single commit of these two changes. Specify the branch name in the commit message!
1. Inspect the working tree


<details>
<summary>🔍 Click here for a hint! </summary>

- To switch between branches use `git switch name_of_branch`
- To inspect the working tree use `git status`
</details>


#### 🚀 Optional challenge

1. Use `nano` to change `second line` to `2nd line`  
1. Use `git diff` to inspect the change before committing 
1. Think about this: when this branch is eventually merged into `main`, will Git
be able to resolve this automatically? Why or why not?


## 3 💪 Explore differences across branches

Follow the steps listed below:

1. Move to branch `b2`
1. Inspect several previous versions in the history using `git diff` with appropriate arguments
1. How far back in the history of branch `b2` can you go?
1. Inspect the differences in `lines.txt` between the latest commit of branch `b2` and branch `b1`
1. Inspect the differences in `lines.txt` between the `main` branch and the parent of the latest commit on branch `b2`

<details>
<summary>🔍 Click here for a hint! </summary>

- To switch between branches use `git switch name_of_branch`
- To inspect different versions in the history use in the current branch use `git diff HEAD~1`
- To inspect differences between branches `git diff name_of_branch name_of_another_branch`
- Add `~1` or `~2` to the name of the branch to refer to earlier commits (e.g. parents)
</details>

#### 🚀 Optional challenge - detect a specific change

1. Use `git log --oneline --all` to list the commits across all branches
1. Use `git diff <hash1> <hash2>` with pairs of commit hashes to narrow down which commit first introduced `fifth line`
1. Confirm your answer with `git show <hash>` — this shows the full content of a single commit


## 4 💪 A first type of merge

Follow the steps listed below:

1. Inspect the differences of `lines.txt` between branches. How many more lines are there in `lines.txt` in `main` than in `b2`? 
1. Merge `b2` into `main`
1. Solve any conflicts
1. Commit the merge
1. Verify the outcome with a graphed `git log`

<details>
<summary>🔍 Click here for a hint! </summary>

- To inspect differences between branches `git diff name_of_branch name_of_another_branch`
- To merge branches, first stand on the branch that will *receive* the changes with `git switch target_branch` and then do the merge with `git merge incoming_branch`
- Use `nano file_in_conflict` to edit the file manually
</details>

#### 🚀 Optional challenge

After the merge is done, look at the graph with `git log --oneline --all --graph`.  
Now try to answer these questions just by reading the graph — without using `cat` or `diff`:

- Which commit introduced the conflict?
- Which branch was ahead before the merge?
- How many parent commits does the merge commit have, and why?

Then verify your answers by inspecting the commits directly with
`git diff <hash1> <hash2>`.


## 5 💪 Interactive Git 

1. Go to this link https://learngitbranching.js.org/
1. Complete **Introduction episodes 1, 2, 3**

#### 🚀 Optional challenge
Continue on the more advanced exercises


## 6 💪 Undo a bad merge

Merges don't always go as planned. In this exercise you will practice how to cancel a merge while it is in progress.

1. Create a new branch called `bad-merge` and step into it
1. Add a new line with the text `tenth line - branch` at the bottom of `lines.txt`
1. Commit your changes
1. Switch back to `main`
1. Add a new line with the text `tenth line - main` at the bottom of `lines.txt`
1. Commit your changes
1. Merge `bad-merge` branch into `main`
1. Use `cat` to see the content of `lines.txt`
1. Imagine you do not know how to solve this conflict and decide to take a step back
1. Use the command `git merge --abort` to cancel the merge process
1. Check the status of git
1. Use `cat` to see the content of `lines.txt`
1. Check the history of git and confirm there was no merge commit created
1. Force delete the `bad-merge` branch using `git branch -D bad-merge`

<details>
<summary>🔍 Click here for a hint! </summary>

- To create use `git branch name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To merge branches, first stand on the branch that will *receive* the changes with `git switch target_branch` and then do the merge with `git merge incoming_branch`
- To check the status of git use `git status`
- To check the history of a branch use `git log --oneline --all --graph`
</details>

#### 🚀 Optional challenge

Now practice undoing a merge that was **already committed**.

1. Recreate steps above until you execute the merge of `bad-merge` into `main`
1. Instead of cancelling the merge, resolve the conflict
1. Commit the merge
1. Verify the merge commit appears in `git log --oneline --graph`
1. Imagine you are not happy with the merge and want to `undo` it
1. Use `git reset --hard HEAD~1` to undo the merge commit
1. Verify the merge commit is gone and `lines.txt` is back to its previous state
1. Force delete the `bad-merge` branch using `git branch -D bad-merge`
>> **Think about this:** what would happen if you had already pushed the merge
   commit to GitHub before running `git reset --hard`?


## 7 🚀 Optional challenge: Full workflow in a GUI

Complete the following tasks using your GUI of choice (eg VS Code, PyCharm, GitHub Desktop, R Studio)

1. Open your GUI of choice
1. Open the folder with your local repository `logbook`
1. Make sure your local `main` is up to date with the remote
1. Create a new branch called `gui` and stand on it
1. Modify the text `second line` to `2nd line`
1. Add a new line with the text `gui line` at the bottom of `lines.txt`
1. Visualize the changes in `lines.txt`
1. Stage and commit the change with message `Add gui line`
1. Switch back to `main` 
1. Add a new line with the text `gui main line` at the bottom of `lines.txt`
1. Visualize the changes in `lines.txt`
1. Stage and commit with message `Add gui main line`
1. Merge `gui` into `main` and resolve the conflict that appears — keep both lines in the file
1. Stage and commit the merge
1. Push `main` to the remote and verify the result on GitHub

Reflections:

- Where does the GUI show you that a conflict exists?
- How does it highlight the conflict markers?
- How do you confirm a file is resolved before committing?
