1\. Git is a tool on your computer that keeps track of changes to files. GitHub is a website where you can store and share Git projects.

2\. A commit is a saved snapshot of your project’s changes, with a short note about what you changed.

3\. The staging area lets you choose which changes go into your next commit. You can leave unfinished or unrelated changes out.

4\. Clone copies a project from a place like GitHub onto your computer. Push sends your commits from your computer to GitHub. Pull brings commits from GitHub down to your computer.

## Commands I used
| Command | What it does |
|---------|--------------|
| `git --version` | Checks Git is installed |
| `git config --global user.name "..."` | Sets the name on my commits |
| `git config --global user.email "..."` | Sets the email on my commits (I used GitHub's private noreply address) |
| `git clone <url>` | Copies a repo from GitHub to my laptop |
| `git status` | Shows what changed and what's staged |
| `git add <file>` | Stages a file for the next commit |
| `git commit -m "message"` | Saves a labelled snapshot |
| `git push` | Uploads my commits to GitHub |
| `git pull` | Downloads new commits from GitHub |
| `git log --oneline` | Shows the commit history, one line each |

## What went wrong and how I fixed it
- I ran `git status` in my home folder instead of the repo folder. It listed my whole user profile as untracked files. I fixed it with `cd devops-learning`. Lesson: check the prompt shows the repo folder before running any Git command.
- I first set my real email in Git config, but my repo is public, so every commit would have shown it. I switched to GitHub's noreply address before my first commit.
