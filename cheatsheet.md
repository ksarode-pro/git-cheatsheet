## The Comprehensive Daily Git Cheatsheet
------------------------------
## 1. Syncing with the Remote Server

## git fetch

* Detailed Explanation:
This command downloads commits, files, and branch tracking references from a remote repository into your local .git directory. It does not modify or overwrite your active working directory or your current code. It simply updates your local Git database's knowledge of what exists on GitHub.
* Detailed Options Used:
* --all: Fetches metadata from all registered remote repositories instead of just the default one.
   * --prune (or -p): Cleans up stale remote-tracking branches. If a coworker deleted a branch on GitHub, this option ensures it is deleted from your local tracking list.
* Detailed Arguments:
* <remote> (Optional): Specifies the nickname of the remote server (e.g., origin). If omitted, Git defaults to the remote associated with your current branch.
* Usage Scenario (Morning Catch-Up):
You log into work at 9:00 AM. Before writing any code, you run git fetch --all --prune. This gives you a clear snapshot of everything your team pushed to GitHub overnight without modifying your local workspace or causing code conflicts.

## git pull

* Detailed Explanation:
This is a compound command that performs a git fetch followed immediately by a git merge (or git rebase) to update your current local branch with changes from the remote server. It actively updates your files.
* Detailed Options Used:
* --rebase: Instead of using a standard merge to integrate remote changes, it unplugs your unpushed local commits, fetches the remote commits, and then replays your commits right on top of the new incoming code. This avoids ugly "Merge branch..." commits.
* Detailed Arguments:
* <remote> (Optional): The remote repository name (e.g., origin).
   * <branch> (Optional): The specific remote stream you want to pull down (e.g., main).
* Usage Scenario (Updating Main Baseline):
You are about to start a new task. You switch to your local main branch and execute git pull origin main --rebase. This ensures that your local starting point is perfectly identical to the latest production code on GitHub.

------------------------------
## 2. Feature Isolation & Branching

## git switch

* Detailed Explanation:
Introduced to make branch navigation safer and clearer, this command updates the files in your working directory to match the version stored on the branch you specify. It also moves your HEAD pointer to that target branch.
* Detailed Options Used:
* -c (or --create): Instructs Git to create a brand-new branch using the name provided, and then immediately switch your workspace into it.
* Detailed Arguments:
* <branch-name> (Required): The name of the branch you want to move into (e.g., feature/payment-gateway).
* Usage Scenario (Starting a Jira Ticket):
You pick up a new task to build an onboarding form. You run git switch -c feature/user-onboarding. Your workspace is instantly isolated from main, allowing you to write code safely without affecting the core application.

------------------------------
## 3. The Local Development & Staging Loop

## git status

* Detailed Explanation:
This is an informational command that scans your current working directory against your Git index (the staging area). It shows you exactly which files have been modified, which files are untracked (new), and which files are staged for your next commit.
* Detailed Options Used:
* -s (or --short): Strips out the conversational text instructions and outputs a compact, two-column status matrix (e.g., M for staged modifications, M for unstaged modifications, ?? for untracked files).
* Detailed Arguments: None.
* Usage Scenario (Sanity Checking):
You have been coding for two hours and lose track of the files you have altered. You run git status -s to get a rapid, clean view of your active workspace before preparing your save point.

## git diff

* Detailed Explanation:
This command runs a differential comparison between two states of your repository. By default, it displays the line-by-line code changes you have typed in your editor that have not yet been added to the staging area.
* Detailed Options Used:
* --staged (or --cached): Changes the target of the comparison. Instead of showing unstaged edits, it shows the exact modifications present inside your staging area compared to your last commit.
* Detailed Arguments:
* <file-path> (Optional): Targets a specific file (e.g., git diff src/components/Button.js). If omitted, it displays changes across the entire project.
* Usage Scenario (Self Code Review):
Before committing your code, you run git diff --staged. This allows you to perform a final review of your own code lines to catch accidental console logs, temporary debugging values, or poor formatting before finalizing the save.

## git add

* Detailed Explanation:
This command moves changes from your working directory into the Git staging area (the index). This action tells Git that you explicitly intend to include these specific alterations in your upcoming project snapshot.
* Detailed Options Used:
* -p (or --patch): Opens an interactive wizard that lets you review modifications in small code blocks ("hunks"). You can choose to stage some lines of a file while leaving other lines out of the upcoming commit.
* Detailed Arguments:
* <path> (Required): The directory or file path to stage. Passing a dot (.) tells Git to recursively scan the entire current folder and stage every single modification or new file found.
* Usage Scenario (Preparing a Save):
You have successfully written unit tests and updated a service file. You execute git add . to package all these working modifications together into the staging area, marking them as ready for a permanent save.

## git commit

* Detailed Explanation:
This command takes everything currently sitting in your staging area and bakes it into a permanent cryptographic snapshot inside your local repository's history. Each commit generates a unique SHA-1 hash tracking ID.
* Detailed Options Used:
* -m "<message>": Bypasses Git's default behavior of opening a system text editor (like Vim or Nano) by allowing you to supply the commit message directly in your terminal line.
   * --amend: Modifies your absolute last commit. It lets you add newly staged changes or rewrite the last commit message without creating an entirely new node in your history log.
* Detailed Arguments:
* "<message>" (Required when using -m): The structured text string explaining why the code was changed (e.g., "feat: integrate Stripe API").
* Usage Scenario (Locking in Progress):
Your feature's API layer is verified and fully functional. You run git commit -m "feat: implement database connection pool" to permanently save your progress before shifting focus to the front-end interface.


