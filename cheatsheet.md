# The Real-World Git Cheatsheet

### Project Scenario Context
For all examples below, imagine you are building an e-commerce storefront web application. You are assigned to a Jira sprint ticket named **`SHOP-42`** to implement a user shopping cart component.

---

## Phase 1: Starting the Workday & Syncing

### 1. `git fetch`
* **Explanation:** Downloads updates from GitHub to your local tracking database without touching your workspace files.
* **Options Used:** `--all` (fetch updates from all registered remotes), `--prune` (remove dead local tracking references of branches deleted on GitHub).
* **Arguments:** `origin` (the explicit name of your primary remote cloud server).
* **Example Call:**
  ```bash
  git fetch origin --all --prune
  ```
* **Scenario:** You log in at 9:00 AM. Your teammate Sarah merged a brand new checkout feature last night. Running this command alerts your local Git client that Sarah's remote branch `feature/checkout` exists, allowing you to track it without altering any code lines on your physical hard drive yet.

### 2. `git pull`
* **Explanation:** Downloads remote changes and instantly merges them into your active branch.
* **Options Used:** `--rebase` (replays your local, unpushed commits on top of incoming remote changes to keep history linear).
* **Arguments:** `origin` (remote name), `main` (branch name).
* **Example Call:**
  ```bash
  git pull origin main --rebase
  ```
* **Scenario:** Before starting your shopping cart feature, you need to make sure your local `main` branch has Sarah's latest checkout code updates. You run this command while on `main` to instantly fast-forward your local text files.

---

## Phase 2: Feature Isolation

### 3. `git switch`
* **Explanation:** Navigates between branches or creates new development lanes.
* **Options Used:** `-c` (creates a brand new branch before executing the switch).
* **Arguments:** `<branch-name>` (the string name you want to give your workspace lane).
* **Example Call:**
  ```bash
  git switch -c feature/SHOP-42-shopping-cart
  ```
* **Scenario:** You are ready to start writing code for the shopping cart interface. You run this command to safely step out of the stable `main` production environment into an isolated development lane named `feature/SHOP-42-shopping-cart`.

---

## Phase 3: Local Development Loop

### 4. `git status`
* **Explanation:** Checks what files have been changed, created, or staged for a snapshot.
* **Options Used:** `-s` (short, clean matrix view).
* **Arguments:** None.
* **Example Call:**
  ```bash
  git status -s
  ```
* **Example Output:**
  ```text
   M src/components/Cart.js
  ?? src/images/cart-icon.png
  ```
* **Scenario:** You just created a brand new shopping cart asset icon file and modified the core Cart logic script. You run this command to double-check that Git safely detects both files before you try to write them to history.

### 5. `git diff`
* **Explanation:** Shows the exact line-by-line code edits you made in your IDE compared to your last commit.
* **Options Used:** None (defaults to un-staged modifications).
* **Arguments:** `<file-path>` (optional; limits the visual breakdown to a single file).
* **Example Call:**
  ```bash
  git diff src/components/Cart.js
  ```
* **Example Output:**
  ```diff
  @@ -10,4 +10,5 @@ const Cart = () => {
   return (
     <div>
       <h1>Your Cart</h1>
  +    <p>Items count: {items.length}</p>
     </div>
   )
  ```
* **Scenario:** Before staging your work, you want to review your code additions to make sure you didn't leave an accidental temporary tracking statement like `console.log("test")` inside `Cart.js`.

### 6. `git add`
* **Explanation:** Moves file edits into the Staging Area, packaging them for an upcoming snapshot.
* **Options Used:** None.
* **Arguments:** `.` (targets every single modified file in the current folder) or explicit file paths.
* **Example Call:**
  ```bash
  git add src/components/Cart.js src/images/cart-icon.png
  ```
* **Scenario:** Your shopping cart UI is fully built. You run this command to explicitly stage both your code implementation file and your icon image asset into the staging index area together.

### 7. `git commit`
* **Explanation:** Permanently saves your staged files as a historic checkpoint inside your local repository.
* **Options Used:** `-m` (directly pass the message string via the terminal command instead of opening an internal editor).
* **Arguments:** `"<message>"` (a descriptive explanation of why the changes were written).
* **Example Call:**
  ```bash
  git commit -m "feat: add cart counter UI component"
  ```
* **Scenario:** Your code is staged and passes local validation checks. You execute this command to lock in your progress, creating a permanent history node labeled with your engineering notes.

---

## Phase 4: Preparing for Code Review (PR)

### 8. `git push`
* **Explanation:** Uploads your local commit history checkpoints to the remote GitHub cloud.
* **Options Used:** `-u` (links local branch to the remote stream for future short pushes).
* **Arguments:** `origin` (remote destination), `feature/SHOP-42-shopping-cart` (branch name).
* **Example Call:**
  ```bash
  git push -u origin feature/SHOP-42-shopping-cart
  ```
* **Scenario:** You finished the task and committed everything locally. You run this command to push your branch up to GitHub, which automatically generates a link in your terminal to open a Pull Request for peer engineering review.

---

## Phase 5: Code Integration & Refreshing

### 9. `git rebase`
* **Explanation:** Unplugs your local feature commits, pulls down new updates from another branch, and replaces your commits cleanly on top of the fresh baseline.
* **Options Used:** None.
* **Arguments:** `origin/main` (the updated remote target baseline branch).
* **Example Call:**
  ```bash
  git rebase origin/main
  ```
* **Scenario:** While you were writing your shopping cart component, a teammate merged a global styling modification into `main`. GitHub warns that your branch now has file conflicts. You run this command to pull their updates directly *underneath* your cart commits so you can resolve the conflict lines locally.

### 10. `git merge`
* **Explanation:** Integrates code branches by generating an explicit "Merge Commit".
* **Options Used:** `--no-ff` (forces a visible history node even if an automatic fast-forward tracking merge is possible).
* **Arguments:** `<branch-name>` (the branch you are ingesting).
* **Example Call:**
  ```bash
  git switch main
  git merge feature/SHOP-42-shopping-cart --no-ff
  ```
* **Scenario:** Your team approved your shopping cart Pull Request on GitHub. You switch over to your local production-ready `main` branch and execute this merge command to bundle your code permanently into the primary release branch history.

---

## Phase 5B: Handling Merge Conflicts

### 11. `git status` (During a Conflict)
* **Explanation:** Identifies exactly which files have overlapping, conflicting changes that Git cannot automatically merge.
* **Options Used:** None.
* **Arguments:** None.
* **Example Call:**
  ```bash
  git status
  ```
* **Example Output:**
  ```text
  Unmerged paths:
    (use "git add <file>..." to mark resolution)
          both modified:   src/components/Cart.js
  ```
* **Scenario:** You ran `git rebase origin/main`, and Git stopped midway, flashing a conflict alert. You run `git status` to see a clean list of files that require your manual code intervention.

### 12. Editing Conflict Markers (The Manual Resolution)
* **Explanation:** Git inserts physical visual anchors inside your code files to separate your changes from the incoming team changes. You must open these files in your editor, choose the correct code lines, and delete the markers.
* **Options/Arguments:** Handled inside your IDE (e.g., VS Code), not the terminal.
* **Example File View (`src/components/Cart.js`):**
  ```javascript
  <<<<<<< HEAD
  const cartTheme = "dark-mode"; // Incoming changes from Main branch
  =======
  const cartTheme = "ocean-blue"; // Your active changes on your Feature branch
  >>>>>>> feature/SHOP-42-shopping-cart
  ```
* **Scenario:** You open `Cart.js` in your editor. Your team decided that the application should use `dark-mode`. You manually delete the `ocean-blue` line and wipe out all three marker lines (`<<<<<<<`, `=======`, `>>>>>>>`), saving only the clean production code.

### 13. `git add` & Continue (Finishing the Resolution)
* **Explanation:** Marks the conflicted files as safely resolved in the staging area and tells Git to proceed with its interrupted process.
* **Options Used:** None.
* **Arguments:** `.` (stages all resolved files) or the specific file path.
* **Action Block:**
  ```bash
  # Step 1: Stage the resolved file
  git add src/components/Cart.js

  # Step 2: Tell Git to continue the rebase or merge process
  git rebase --continue
  ```
* **Scenario:** After cleaning up the conflict markers in your code editor, you run `git add .` to let Git know the file is safe. You then execute `git rebase --continue` to let Git successfully apply the rest of your commits.

---

## Phase 6: Cleanup & Workspace Maintenance

### 14. `git branch`
* **Explanation:** Views, tracks, or destroys workspace branches.
* **Options Used:** `-d` (safely deletes a branch only if it has been fully merged elsewhere).
* **Arguments:** `<branch-name>` (the target branch to erase).
* **Example Call:**
  ```bash
  git branch -d feature/SHOP-42-shopping-cart
  ```
* **Scenario:** Your shopping cart code is fully merged and live on production. You switch back to `main` and run this command to erase the local copy of your feature branch from your computer storage to keep your terminal list clean.

---

## Phase 7: Emergency Toolkit & Recovery

### 15. `git stash`
* **Explanation:** A temporary safe-deposit box that hides uncommitted local edits so your workspace becomes instantly identical to your last commit.
* **Options Used:** `-u` (includes newly created, untracked files in the hideaway action).
* **Arguments:** `pop` (restores the last hidden item block to your editor workspace).
* **Example Call:**
  ```bash
  git stash -u
  # ... switch branches, fix a bug, switch back ...
  git stash pop
  ```
* **Scenario:** You are halfway through editing a complex layout script for the cart when an urgent alert comes in that production is down. You don't want to lose your half-written work, so you run `git stash -u` to clear your workspace. You switch to `main`, patch the production bug, switch back, and run `git stash pop` to resume work.

### 16. `git reset`
* **Explanation:** Rewinds your active repository branch pointer to a specific point in your past history, undoing commits.
* **Options Used:** `--soft` (undoes commits but keeps your actual text edits open and staged inside your code editor).
* **Arguments:** `HEAD~1` (specifies rewinding exactly one commit back from where you stand).
* **Example Call:**
  ```bash
  git reset --soft HEAD~1
  ```
* **Scenario:** You ran a commit but immediately realized you forgot to include an export block at the bottom of your script file. Instead of making a secondary fix commit, you run this command to erase that last local commit node, type out the correction, and commit cleanly.

### 17. `git merge --abort`
* **Explanation:** Instantly cancels an active merge process that has stalled due to conflicts, completely rolling back your files to the state they were in before you initialized the merge.
* **Options Used:** `--abort` (the safety flag that triggers the cleanup and escape process).
* **Arguments:** None.
* **Example Call:**
  ```bash
  git merge --abort
  ```
* **Scenario:** You run `git merge feature/analytics` into your branch, and it throws 45 complicated merge conflicts across critical architecture files. You realize you don't have the time or context to solve them right now. You run this command to instantly clean up the mess and bring your workspace back to safety.

### 18. `git rebase --abort`
* **Explanation:** Instantly cancels an active rebase operation that is stuck on conflicts. It rolls back your branch pointer and restores your local commits to exactly how they looked before you tried to rebase.
* **Options Used:** `--abort` (the safety cancellation flag).
* **Arguments:** None.
* **Example Call:**
  ```bash
  git rebase --abort
  ```
* **Scenario:** You are middle-way through a multi-commit rebase (`git rebase origin/main`) and get terribly confused fixing conflict markers on commit 3 of 7. To prevent corrupting your codebase history, you run this command to escape safely. You can then regroup and try again cleanly.
