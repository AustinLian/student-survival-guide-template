# CSUS Git & GitHub Workshop

**Western Student Survival Guide | December 1, 2026 | 6:30–8:00 PM**

By the end, you will have cloned a real repository, committed your own changes, opened and reviewed a pull request, and resolved a merge conflict with a teammate. No programming experience is required for the Markdown exercises.

## 0. Before the workshop

- Create a [GitHub account](https://github.com/signup) and sign in to GitHub in your browser.
- Install [Git](https://git-scm.com/downloads) and [VS Code](https://code.visualstudio.com/download). Reopen VS Code after installation.
- Bring your laptop and charger. On Windows use PowerShell or Git Bash; on macOS/Linux use Terminal. In VS Code, select **Terminal → New Terminal**.

Run these commands individually in your terminal. Replace `Your Name` and `YOUR_EMAIL` with your own name and a GitHub-verified email (or your GitHub no-reply email from Settings → Emails). Keep the quotation marks.

```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "YOUR_EMAIL"
git config --global user.name
git config --global user.email
```

**Check:** a Git version and your configured name/email appear. Git identity is separate from GitHub authentication. For HTTPS pushes, sign in through the browser credential prompt if available. GitHub account passwords are not accepted for Git operations. Ask a helper if authentication fails; never share a token or password.

## 1. Git vs. GitHub

Git records versions of files on your computer. GitHub hosts repositories online so teams can share work. A repository contains files and Git history. A commit records a snapshot of staged changes. A branch is a separate line of development; `main` is the shared version of our project.

**Working files → `git add` → staging area → `git commit` → local history → `git push` → GitHub**. `git pull` brings remote changes into your current branch. A pull request (PR) is a request to merge changes into another branch; it is created on GitHub, not with `git pull`.

## 2. Create your team repository (team leader only)

1. Form a group of 2–3 and choose one leader. Assign each group a team number.
2. Open the instructor's public starter template link. If you are reading this guide in the starter template on GitHub, you are already there.
3. Click **Use this template → Create a new repository**. Choose your personal account as owner, name the repository `student-survival-guide-team-01` (replace `01` with your team number), select **Private**, leave Include all branches unchecked, and create it.
4. Open the NEW team repository (not the instructor's template). Go to **Settings → Collaborators → Add people** (labels may vary). Search each teammate's exact GitHub username and send invitations.
5. Teammates accept the invitation from GitHub/email. The leader opens **Code → HTTPS → Copy URL** and shares this URL with the group. Everyone must clone the same team repository. Do not fork or download the ZIP.

**Check:** all teammates can open the private team repository while signed in. The files `README.md`, `meetup.md`, `.gitignore` and this guide should appear if the instructor has added the guide to the template.

## 3. Clone the repository (everyone)

Open a terminal in your usual projects folder (not inside another Git repository). The commands below use a fictional team leader, `TEAM_LEADER`, and team 01. **Replace `TEAM_LEADER` and `01` with your team's actual values, or paste the copied HTTPS URL after `git clone`.** Run one command per line. The `cd` folder must match the actual repository name.

```bash
git clone https://github.com/TEAM_LEADER/student-survival-guide-team-01.git
cd student-survival-guide-team-01
git status
git remote -v
git log --oneline -5
```

Open the cloned folder in VS Code: `code .` if available, otherwise File → Open Folder and choose the cloned folder. Use the VS Code terminal in that folder for the rest of the workshop.

**Check:** `git status` shows branch `main` and a clean working tree. `origin` in `git remote -v` points to your TEAM repository, not the instructor's template.

## 4. Make your own branch and first commit (everyone)

Choose a unique lowercase identifier (e.g. your first name plus initial). The example below uses `alex` and a study topic. Replace `alex` consistently in your branch and filename with your identifier. Each teammate must use a different branch and file. Leave `meetup.md` unchanged for now.

```bash
git switch main
git pull --ff-only
git switch -c add-alex-tips
git status
```

In the VS Code Explorer, click New File inside the cloned folder. Name it `tips-alex.md` (use your identifier). Write 2–3 student tips in Markdown, such as:

```markdown
# Study tips
- Find a quiet study space before exams.
- Bring a charger to the library.
```

Save the file (Ctrl+S on Windows/Linux, Cmd+S on macOS). Then run:

```bash
git status
git add tips-alex.md
git diff --staged
git commit -m "Add student tips"
git status
git log --oneline -5
```

**Check:** before `git add`, your new file is untracked. After adding it, it is staged. After committing, you have a clean working tree and a new commit on your branch. `git diff` shows unstaged edits to tracked files; it does not show the contents of brand-new untracked files. `git diff --staged` shows what you are about to commit. Press `q` if Git opens a pager.

## 5. Push your branch (everyone)

```bash
git push -u origin add-alex-tips
```

Use your own exact branch name. `-u` sets the upstream; later pushes on that branch can be simply `git push`.

**Check:** refresh the team repo on GitHub; your branch should be listed. If you see an authentication or permission error, verify the correct GitHub account and accepted invitation before trying again.

## 6. Open, review and merge pull requests (everyone)

### Author
1. Open your team repository on GitHub. Click **Compare & pull request** if shown, or **Pull requests → New pull request**.
2. Base: `main` (destination). Compare: `add-alex-tips` (your branch).
3. Title: `Add student tips`. Description: what you added and how you checked it.
4. Click **Create pull request** and send the PR link to a teammate.

### Reviewer (not the author)
1. Open your teammate's PR. Click **Files changed** and check that their file contains useful tips and no sensitive data.
2. Choose **Review changes**; leave a specific comment or approve, then submit the review. Example: “The tips are clear. The filename is unique.” You cannot approve your own PR.
3. Once reviewed, an authorized teammate or the owner clicks **Merge pull request → Confirm merge**. Use a merge commit if available. Repeat until all contributions are merged. Do not delete branches that still have unmerged work.

### Everyone: update your own local main

```bash
git switch main
git pull --ff-only
git status
git log --oneline -5
```

**Check:** the team repository's `main` and your local folder contain each student's unique `tips-*.md` file. The first commit, branch, push and PR together count as your contribution; there is no need to create a second tips branch.

## 7. Team challenge: intentionally create a merge conflict

A conflict happens when Git cannot automatically combine competing edits to the same part of a file. Pick two teammates, A and B. A third teammate can review and help. Both must start from the same updated `main` with exactly `Meetup: TBD` in `meetup.md`. **Do not merge either conflict PR until BOTH branches are created, edited, committed and pushed.**

### A: make your change

```bash
git switch main
git pull --ff-only
git switch -c conflict-a
```

Open `meetup.md` in VS Code. Replace the entire line with `Meetup: Taylor Library`, then save.

```bash
git add meetup.md
git commit -m "Set meetup to Taylor Library"
git push -u origin conflict-a
```

### B: make a different change (before A merges)

```bash
git switch main
git pull --ff-only
git switch -c conflict-b
```

Open `meetup.md`. Replace the entire line with `Meetup: Weldon Library`, then save.

```bash
git add meetup.md
git commit -m "Set meetup to Weldon Library"
git push -u origin conflict-b
```

### Both: open PRs

Create two PRs into `main`, one from `conflict-a` and one from `conflict-b`. Once both exist, review and merge **A's PR only**. B's PR should now report a conflict. If it doesn't, check that both people edited the same original line before A merged.

### B: resolve locally

Stay in your own clone. Start with a clean working tree on `conflict-b`.

```bash
git switch conflict-b
git status
git fetch origin
git merge origin/main
git status
```

The merge reports a conflict in `meetup.md`. Open the file. You may see markers like these (labels can vary):

```text
<<<<<<< HEAD
Meetup: Weldon Library
=======
Meetup: Taylor Library
>>>>>>> origin/main
```

The team agrees on one final version. For example, replace the whole block with just `Meetup: Weldon Library`. Remove ALL markers and the unwanted line. Save, then run:

```bash
git add meetup.md
git diff --staged
git commit -m "Resolve meetup conflict"
git push
git status
```

If `git diff --staged` is empty because your chosen text matches your branch, the merge still needs a commit. Check `git status` for “all conflicts fixed but you are still merging”. If you get stuck before committing, `git merge --abort` cancels the in-progress merge (start clean). Ask a helper rather than force pushing or resetting.

Refresh B's PR, review the resolved file, and merge it. Everyone syncs:

```bash
git switch main
git pull --ff-only
git status
```

**Final check:** all tips files exist, `meetup.md` has one agreed line with no markers, and everyone's working tree is clean.

## 8. Quick command reference

| Command | What it does |
| --- | --- |
| `git init` | Creates a Git repository in the current folder (not needed for a clone). |
| `git clone URL` | Copies a remote repo and its history into a new folder. |
| `git status` | Shows current branch and file state. Use it often. |
| `git remote -v` | Shows remote URLs. |
| `git diff` | Shows unstaged changes to tracked files. |
| `git add FILE` | Stages a file's current content. |
| `git diff --staged` | Shows staged changes. |
| `git commit -m "MESSAGE"` | Records the staged snapshot locally. |
| `git log --oneline -5` | Shows up to five recent commits. |
| `git branch` | Lists local branches; `*` marks the current branch. |
| `git switch -c NAME` | Creates and switches to a branch. |
| `git switch main` | Switches to the existing main branch. |
| `git push -u origin NAME` | Publishes a branch and sets its upstream. |
| `git push` | Uploads commits to the tracked remote branch. |
| `git pull --ff-only` | Updates the current branch only when fast-forward is possible. |
| `git fetch origin` | Downloads remote history without merging. |
| `git merge origin/main` | Integrates the fetched remote main into your current branch. |
| `git restore --staged FILE` | Unstages a file without discarding edits. |
| `git merge --abort` | Cancels an in-progress merge before committing. |

## 9. Troubleshooting

| Symptom | What to do |
| --- | --- |
| `git` not recognized | Install Git and reopen your terminal; run `git --version`. |
| `not a git repository` | Run `pwd` or `Get-Location`; `cd` into the cloned project. |
| Repository not found / permission denied | Verify the URL, correct account and accepted collaborator invitation. |
| `Author identity unknown` | Configure `user.name` and `user.email` as in step 0. |
| Nothing to commit | Save the file, run `git status`, stage the intended file. |
| Wrong branch | Check `git status` and `git branch`. Preserve uncommitted work before switching. |
| No upstream | `git push -u origin YOUR_BRANCH` (substitute your branch). |
| Push rejected / non-fast-forward | Fetch, inspect and ask a helper if histories diverge. Do not force push. |
| `main` will not fast-forward | Stop and ask a helper; you may have committed directly to main. |
| Staged wrong file | `git restore --staged FILE` keeps your working edits. |
| Conflict markers | Edit affected file, remove markers, stage, commit, push. |
| Secret accidentally committed | Stop, notify the repo owner, rotate/revoke it. `.gitignore` does not erase history. |

## 10. After the workshop

- Make small, meaningful commits: `Add study tips`, not `stuff`.
- Pull main before branching; avoid direct commits to shared main. Review teammates' PRs.
- `.gitignore` prevents matching untracked files from being added accidentally (e.g. `.env`, `node_modules/`). It does not untrack already committed files. Never commit secrets.
- CI (continuous integration) runs automated checks such as tests; CD (continuous delivery/deployment) automates release or deployment when configured. GitHub Actions can run checks on PRs.

Continue practising: [Pro Git](https://git-scm.com/book/en/v2), [GitHub Skills](https://skills.github.com/), [Learn Git Branching](https://learngitbranching.js.org/), [GitHub PR documentation](https://docs.github.com/en/pull-requests), [GitHub Actions](https://docs.github.com/en/actions).
