# Studio N

## Git Fundamentals

In this studio, you will practice the core git workflow you will use throughout the course: cloning a repository, deciding which files git should and should not track, inspecting changes you have made, undoing changes you did not mean to make, and staging, committing, and pushing your work.

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

Record your answers in `ANSWERS.md` as you work. Number your responses so they are easy to match to the exercises.

1. Clone your `studioN` repo and change into the cloned directory:

   ```
   git clone <your-repo-url>
   cd studioN
   ```

   Check the current state of the repository:

   ```
   git status
   ```

   In your answers, show:

   - the command you used to clone the repo
   - the output of `git status` immediately after cloning

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

   In your answers:

   - show the `git status` output and explain why one new file appears in it and the other does not
   - add one more pattern of your own to `.gitignore` that would exclude a type of file you expect to generate later in the course (for example, `*.o` or `*.swp`). You can append it directly from the command line:

     ```
     echo "*.o" >> .gitignore
     ```

     Show the line you added and confirm with `cat .gitignore`.

3. Open `notes.txt` in a text editor (or append to it from the command line) and add two or three sentences to it:

   ```
   echo "Adding a note about today's studio." >> notes.txt
   ```

   View exactly what changed in the tracked file since the last commit, without staging anything first:

   ```
   git diff
   ```

   In your answers, show:

   - the command(s) you used to edit the file
   - the `git diff` output it produced

4. Without staging the change from Exercise 3, discard it and restore `notes.txt` to its last committed state:

   ```
   git restore notes.txt
   ```

   (If your version of git is older and does not support `restore`, use `git checkout -- notes.txt` instead.)

   Confirm the file is back to its original state:

   ```
   git status
   git diff
   ```

   In your answers:

   - show the command you used to discard the change
   - show the `git status` output confirming the working directory is clean
   - briefly explain, in your own words, why this command is dangerous to run without checking `git status` or `git diff` first

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

   In your answers, show:

   - the `git add` command you used
   - the `git status` output showing the change staged
   - the `git commit` command and message you used

6. Make a small edit to a different tracked file in the repo (for example, `progress.txt`) and stage it:

   ```
   echo "Studio in progress." >> progress.txt
   git add progress.txt
   ```

   Before committing, decide you are not ready to commit it after all, and remove it from the staging area *without* discarding the edit from the working directory:

   ```
   git restore --staged progress.txt
   ```

   (On older git versions: `git reset HEAD progress.txt`)

   Check status before and after unstaging:

   ```
   git status
   ```

   In your answers:

   - show both `git status` outputs (staged, then unstaged)
   - explain the difference between the working directory, the staging area, and a commit, based on what you just observed

7. Stage and commit the change from Exercise 6 with a meaningful message:

   ```
   git add progress.txt
   git commit -m "Track studio progress notes"
   ```

   View the commit history in a compact, one-line-per-commit form:

   ```
   git log --oneline
   ```

   In your answers, show:

   - the log command you used
   - the resulting output, which should include at least the two commits you made in Exercises 5 and 7

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

   In your answers:

   - show the command(s) you used and the resulting output
   - explain briefly how `git revert` differs from `git reset <commit-hash>`, and why you might prefer one over the other when working on a shared repo

9. Push your commits to the remote repository:

   ```
   git push
   ```

   Confirm the push succeeded by fetching and checking your local branch against the remote:

   ```
   git fetch
   git status
   ```

   (`git status` should report that your branch is up to date with the remote after fetching.) In your answers, show:

   - the command you used to push
   - the commands and output you used to confirm the push succeeded

## Deliverables

Commit and push all modified and added files, including `ANSWERS.md`, to the repo.
