# Git Fundamentals

## Overview
In this studio, you will practice the core git workflow you will use throughout the course: cloning a repository, deciding which files git should and should not track, inspecting changes you have made, undoing changes you did not mean to make, and staging, committing, and pushing your work.

As you go, you'll build up `ANSWERS.md` into a quick-reference sheet of git commands you can print out and keep next to your keyboard.

## Collaboration

You may complete this studio individually or in a small group.

## Environment Setup

Before starting the exercises, set up a working folder and a terminal you can run git commands from.

**All students:**

1. Create a folder on your computer for this class (for example, `cse4208` or `software-engineering`). You will keep all of your studio repos inside this folder.
2. Open that folder in Visual Studio Code (or Visual Studio). In VS Code: `File > Open Folder...` and select the folder you just created.
3. Open a terminal inside the editor: `Terminal > New Terminal` (or the keyboard shortcut `` Ctrl+` ``). You will run all of the commands in this studio from that terminal.

**Windows users:**

- If you do not already have Git for Windows installed, download and install it from [https://gitforwindows.org](https://gitforwindows.org), accepting the default options.
- Git for Windows installs Git Bash, which gives you a Unix-style shell (so commands like `ls`, `cat`, and `touch` work as written in this studio). In VS Code, open the terminal dropdown (the small `v` next to the `+` in the terminal panel) and select **Git Bash** as your default shell if it is not selected already.
- Confirm the install by running the following in your Git Bash terminal:

  ```
  git --version
  ```

**Linux users:**

- Most Linux distributions come with git preinstalled. Confirm this by running:

  ```
  git --version
  ```

- If it is not installed, install it with your distribution's package manager, for example on Ubuntu/Debian:

  ```
  sudo apt update
  sudo apt install git
  ```

- The default terminal shell on Linux already supports the Unix-style commands used in this studio, so no additional setup is needed.

**All students, once git is installed:**

If this is your first time using git on this machine, configure your name and email (used to label your commits):

```
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

## Exercises

For each exercise below, run the commands shown, then fill in the matching entry in `ANSWERS.md`. Rather than pasting raw terminal output, write a short explanation **in your own words** of what the command does and what you observed. Treat `ANSWERS.md` as a reference sheet you're building for yourself — write it the way you'd want to read it later.

1. Clone your `studioN` repo and change into the cloned directory:

   ```
   git clone <your-repo-url>
   cd studioN
   ```

   Check the current state of the repository:

   ```
   git status
   ```

   In `ANSWERS.md`, explain what `git status` told you about the state of the repo immediately after cloning, and what a "clean" status means.

2. List all files in the repo, including hidden ones, and view the contents of `.gitignore`:

   ```
   ls -a
   cat .gitignore
   ```

   Create a new empty file whose name matches one of the patterns already listed in `.gitignore` (for example, a file ending in an extension already excluded), and a second empty file whose name does *not* match any pattern:

   ```
   touch build.log
   touch important.txt
   ```

   Check status again:

   ```
   git status
   ```

   In `ANSWERS.md`, explain in your own words what `.gitignore` is for, and why `build.log` and `important.txt` were treated differently by `git status`.

3. Open `notes.txt` in a text editor (or append to it from the command line) and add two or three sentences to it:

   ```
   echo "Adding a note about today's studio." >> notes.txt
   ```

   View exactly what changed in that one tracked file since the last commit, without staging anything first:

   ```
   git diff notes.txt
   ```

   Now run `git diff` with no file argument:

   ```
   git diff
   ```

   Since you have also been editing `ANSWERS.md` as you go, this second command will likely also show changes to `ANSWERS.md`.

   In `ANSWERS.md`, explain what `git diff` shows you in general, and the difference between running it with a filename versus with no argument at all, based on what you just observed.

4. Without staging the change from Exercise 3, discard it and restore `notes.txt` to its last committed state:

   ```
   git restore notes.txt
   ```

   (If your version of git is older and does not support `restore`, use `git checkout -- notes.txt` instead.)

   Confirm the file is back to its original state:

   ```
   git status
   git diff notes.txt
   ```

   In `ANSWERS.md`, explain what `git restore <file>` does, and why it's risky to run without checking `git status` or `git diff` first.

5. Add the same two or three sentences to `notes.txt` again:

   ```
   echo "Adding a note about today's studio." >> notes.txt
   ```

   Stage the change:

   ```
   git add notes.txt
   ```

   Confirm it is staged:

   ```
   git status
   ```

   Commit the change with a message that describes *what* changed and *why*, not just "updated notes.txt":

   ```
   git commit -m "Add reflection notes from git studio exercise 5"
   ```

   In `ANSWERS.md`, explain what `git add` and `git commit` each do, and what makes a commit message "meaningful" rather than generic.

6. Make a small edit to a different tracked file in the repo (for example, `progress.txt`) and stage it:

   ```
   echo "Studio in progress." >> progress.txt
   git add progress.txt
   ```

   Check status now, while the change is staged:

   ```
   git status
   ```

   Before committing, decide you are not ready to commit it after all, and remove it from the staging area *without* discarding the edit from the working directory:

   ```
   git restore --staged progress.txt
   ```

   (On older git versions: `git reset HEAD progress.txt`)

   Check status again, now that the change has been unstaged:

   ```
   git status
   ```

   In `ANSWERS.md`, explain the difference between the working directory, the staging area, and a commit, based on what you just observed.

7. Stage and commit the change from Exercise 6 with a meaningful message:

   ```
   git add progress.txt
   git commit -m "Track studio progress notes"
   ```

   View the commit history in a compact, one-line-per-commit form:

   ```
   git log --oneline
   ```

   In `ANSWERS.md`, explain what `git log --oneline` shows you and why it's useful, referencing what you saw for your own commit history.

8. Suppose you decide the commit from Exercise 7 should not have been made, but you want to preserve history rather than rewrite it. First find the commit hash from your `git log --oneline` output, then create a new commit that undoes the changes introduced by that commit:

   ```
   git log --oneline
   git revert <commit-hash-from-exercise-7>
   ```

   git will open an editor for the revert commit message; save and close it to complete the revert (in `vim`, type `:wq` and press Enter; in `nano`, press `Ctrl+O`, `Enter`, then `Ctrl+X`).

   View the log again:

   ```
   git log --oneline
   ```

   In `ANSWERS.md`, explain what `git revert` does, based on how your `git log --oneline` output changed — did it remove the Exercise 7 commit from history, or add a new commit on top of it? — and why that behavior makes `git revert` safer to use than editing or removing commits directly when working on a shared repo.

9. Before pushing, check what else is still uncommitted — this should include your `.gitignore` changes from Exercise 2 and the `important.txt` file you created:

   ```
   git status
   ```

   Stage and commit those with a meaningful message. Leave `ANSWERS.md` out of this commit — you're still writing into it, including your answers to this very exercise, so you'll commit and push it separately as your last step (see Deliverables below):

   ```
   git add .gitignore important.txt
   git commit -m "Add tracked test file"
   ```

   Now push all of your commits to the remote repository:

   ```
   git push
   ```

   Confirm the push succeeded by fetching and checking your local branch against the remote:

   ```
   git fetch
   git status
   ```

   In `ANSWERS.md`, explain what `git push` and `git fetch` each do, and how you could tell from `git status` that your push succeeded.

## Deliverables

Once you've finished writing all of your answers, including your answers to Exercise 9 itself, stage, commit, and push `ANSWERS.md` one last time:

```
git add ANSWERS.md
git commit -m "Add final studio answers"
git push
```

Commit and push all modified and added files, including `ANSWERS.md`, to the repo.
