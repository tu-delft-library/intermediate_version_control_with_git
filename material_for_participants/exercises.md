# Exercises


## 💪 Get familiar with branches

Perform the following tasks individually:

1. Revise the commands we have used so far
1. Create at least three branches with any name you like
1. Rename them and switch between them
1. Check the status message and the history in each branch you are in

When you are finished:
1. If you have a branch `master`, rename it to `main`
1. Return to the branch `main`
1. Delete all other branches

<details>
<summary>🔍 Click here for a hint! </summary>

- To create use `git branch name_of_branch`
- To rename use `git branch -m current_name_of_branch new_name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To check the history of a branch use `git log --oneline`
- To delete branches use `git branch -d name_of_branch name_of_another_branch`
</details>

## 💪 Commit on a secondary branch

Perform the following tasks individually:

1. Move to branch `b2` (the file `lines.txt` contains three lines)
1. Append a forth and a fifth line using `echo`
1. Make a single commit of these two changes. Specify the branch name in the commit message!
1. Inspect the working tree


## 💪 Explore differences across branches
Perform the following tasks individually:

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



## 💪 A first type for merge
Please perform the following tasks individually

1. How many more lines has `lines.txt` in `main` than in `b2`? 
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


## 💪 Interactive Git 
- Go to this link https://learngitbranching.js.org/
- Complete **Introduction episodes 1, 2, 3**
[Optional] Continue on the more advanced exercises


##  💪 Another type of merge: squash merge

Sometimes a feature branch has many small, messy commits ("fix typo", "try again", "finally works"). A **squash merge** collapses all of them into a single clean commit on `main`, keeping the history readable.

1. Make a new branch called `messy` and stand on it
1. Add 'draft line A' to `lines.txt`
1. Commit your changes
1. Add 'draft line B' to `lines.txt`
1. Commit your changes
1. Add 'final feature line' to `lines.txt`
1. Commit your changes
1. Verify your three commits on `messy`

Here comes the new part:

1. Stand on main branch
1. Merge the `messy` branch by adding the flag `--squash` BEFORE the branch name 
1. Commit your changes
1. Verify the squash commit is in the history

Finally, because no merge commit is created, Git does not know `messy` was merged. 
Clean up explicitly by deleting the branch:
1. Delete `messy`
1. Verify 
1. Push to remote

<details>
<summary>🔍 Click here for a hint! </summary>

- To create use `git branch name_of_branch`
- To switch between branches use `git switch name_of_branch`
- To check the history of a branch use `git log --oneline`
- To delete branches use `git branch -d name_of_branch name_of_another_branch`
- To do a squash commit use `git merge --squash name_of_branch`
</details>


##  💪 Full workflow in a GUI

Complete the following tasks using your GUI of choice (VS Code or GitHub Desktop)

1. Open your GUI of choice
1. Open the folder with your local repository `git-one`
1. Make sure your local `main` is up to date with the remote
1. Create a new branch called `gui` and stand on it
1. Add a new line with the text `gui feature line` at the bottom of `lines.txt`
1. Visualize the changes in `lines.txt`
1. Stage and commit the change with message `Add gui feature line`
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