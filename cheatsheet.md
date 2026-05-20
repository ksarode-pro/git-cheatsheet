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

------------------------------
## 4. Preparing for Code Review (Pull Request / PR)

## git push

* Detailed Explanation:
This command transfers your local commit history from a specific branch directly to your remote repository hosting platform (like GitHub, GitLab, or Bitbucket). It updates the remote references so your teammates can access, review, and test your code lines.
* Detailed Options Used:
* -u (or --set-upstream): Creates a permanent structural tracking link between your local branch and the branch on the remote server. Once run with this flag, you can simply type git push or git pull in the future without specifying the remote or branch names.
   * --force-with-lease: This is a safer variation of the destructive --force flag. It overwrites the remote branch history with your local history, but only if no one else has pushed new commits to that remote branch in the meantime. It protects your teammates' work from being accidentally wiped out.
* Detailed Arguments:
* <remote> (Required on first push): The designated nickname of your cloud server (e.g., origin).
   * <branch> (Required on first push): The exact name of the branch you want to publish to the cloud (e.g., feature/login-validation).
* Usage Scenario (Publishing Code for Review):
You have finished building and testing your feature locally. You run git push -u origin feature/login-validation to upload your branch to GitHub. This actions the generation of a link in your terminal to instantly open a Pull Request (PR) for your team's engineering review.

------------------------------
## 5. Code Integration & Keeping Branches Fresh

## git rebase

* Detailed Explanation:
This command reapplies commits from your current feature branch onto a updated base commit from another branch. It temporarily shelves your feature commits, pulls in the new baseline updates, and then replays your feature commits one by one on top of the newest code, resulting in a perfectly straight, linear history.
* Detailed Options Used:
* -i (or --interactive): Launches a text-based terminal wizard that lets you rewrite your commit history before sharing it. You can combine small commits together (squash), edit past commit messages (reword), or completely remove problematic commits (drop).
* Detailed Arguments:
* <base-branch> (Required): The target branch containing the new code updates you want to pull underneath your work (e.g., origin/main).
* Usage Scenario (Fixing Merge Conflicts Before Merge):
Your Pull Request is blocked on GitHub because another developer merged their code into main ahead of you, creating a conflict. You switch to your feature branch and run git rebase origin/main. You fix the overlapping lines directly in your editor, run git rebase --continue, and achieve a clean, conflict-free PR history.

## git merge

* Detailed Explanation:
This command combines independent lines of development into your active branch. It looks for the common ancestor commit between your current branch and the target branch, then ties their histories together by creating a special, single "Merge Commit."
* Detailed Options Used:
* --no-ff (No Fast-Forward): Forces Git to generate a physical merge commit node even if the merge could be handled automatically without one. This preserves a clear visual diagram showing that a specific feature branch existed and was integrated at that exact moment.
* Detailed Arguments:
* <branch-to-merge> (Required): The target branch whose code you want to pull directly into your active workspace (e.g., feature/analytics).
* Usage Scenario (Deploying Code to Production):
Your team uses a manual merge strategy for release management. You switch your terminal over to the production-ready main branch using git switch main. You then execute git merge feature/analytics --no-ff to safely ingest the finished code into the production build history.

------------------------------
## 6. Local Workspace Clean Up

## git branch

* Detailed Explanation:
This is your primary tool for managing branch lines. Without options, it acts as a simple viewing list. With options, it becomes a cleanup tool to delete dead development environments from your system storage.
* Detailed Options Used:
* -d (or --delete): Deletes the specified branch from your computer. This is a "safe" delete; Git will stop you and throw an error if the branch contains work that has not been safely merged into another major branch yet.
   * -D: A forceful override deletion option. It instructs Git to completely destroy the local branch and its associated commits, regardless of whether that work was saved or merged anywhere else.
* Detailed Arguments:
* <branch-name> (Required for deletion): The exact string name of the branch you want to erase (e.g., feature/old-tab-layout).
* Usage Scenario (Post-PR Housekeeping):
Your user-onboarding Pull Request has been approved, squash-merged on GitHub, and deleted from the online repository. To clean up your local terminal workspace and free up system clarity, you switch to main and execute git branch -d feature/user-onboarding.

------------------------------
## 7. The "Emergency" Workspace Toolkit

## git stash

* Detailed Explanation:
This command acts as a temporary safe-deposit box. It takes all your uncommitted local edits (both staged and unstaged files), saves them onto an internal stack, and leaves you with a completely clean working directory that matches your last official commit.
* Detailed Options Used:
* -u (or --include-untracked): Instructs Git to stash not only modified files but also brand-new files you just created that haven't been tracked by Git yet.
* Detailed Arguments:
* save "<optional-message>": Saves your work to the stash stack with a custom note.
   * pop: Re-applies the most recently stashed changes back onto your working directory and immediately deletes that entry from the stash stack.
   * list: Shows all your saved stashes with their corresponding index numbers (e.g., stash@{0}).
* Usage Scenario (Context Switching for a Hotfix):
You are deep into writing a complex feature, and your code is completely broken and untestable. Suddenly, a critical bug breaks production, and you must fix it immediately. You execute git stash -u to safely hide away your broken experimental code. You switch to main, fix the hotfix, push it, switch back to your feature branch, and run git stash pop to resume exactly where you left off.

## git reset

* Detailed Explanation:
This command resets your current branch pointer to a specific point in your past history, effectively undoing commits. It changes the state of your repository radically depending on the structural flag chosen.
* Detailed Options Used:
* --soft: Undoes your past commits, but leaves your actual code changes completely untouched and sitting inside your staging area, ready to be edited or re-committed.
   * --hard: A destructive option. It undoes your past commits and completely deletes every single line of code change associated with those commits from your physical hard drive, reverting your files exactly to how they looked at that past snapshot point.
* Detailed Arguments:
* <commit-reference> (Required): The target destination indicator. This can be a raw commit SHA hash ID (like a1b2c3d) or a relative pointer like HEAD~1 (which means "exactly one commit prior to where I am standing right now").
* Usage Scenario (Undoing a Faulty Commit):
You make a commit locally, but immediately realize you accidentally left your private API security keys or a glaring syntax typo in the code. You run git reset --soft HEAD~1. The bad commit is instantly erased from your history, but your code remains open in your IDE so you can securely clean out the sensitive keys and commit it correctly.

------------------------------
