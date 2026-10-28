Merge Conflicts and .gitignore: Day 6 Notes

Merge conflicts

\- A merge conflict happens when two branches change the same line differently and Git can’t choose between them.

\- Git stops the merge and marks the file:

<<<<<<< HEAD

Networking is fun.

=======

Networking is hard.

>>>>>>> branch-b

\- Between <<<<<<< HEAD and ======= is the version on my current branch. Between ======= and >>>>>>> is the incoming version.

\- To fix it, I edit the file to keep the right content, delete all three marker lines, then run git add and git commit.

What I did: I created a practice file, changed line 2 differently on branch-a and branch-b, merged both, got a conflict, fixed it by hand and committed. I never pushed it, so my repo stayed clean.



What a conflict feels like (own words): It feels like Git is asking me to make a choice because it can’t tell which change I want. Once I understand what each version does, I can combine them or keep the one that makes sense.

.gitignore

\- A .gitignore file lists files Git should not track, such as secrets, logs and generated folders.

\- Mine ignores .env, \*.pem, \*.key, \_\_pycache\_\_/, \*.pyc, .venv/, \*.log and Thumbs.db.

\- I tested it by creating a fake .env file. git status didn’t list it, and git check-ignore -v .env showed which rule matched.

\- .gitignore only protects files that haven’t been committed yet. If a secret is committed, it stays in the history, so I’d have to rotate the password or key.

Why this matters for me (own words): I want to keep private information like passwords and keys out of my repository. Ignoring generated files also keeps my repo tidier and makes it easier to see the changes I actually care about.

Commands I used

Command	What it does

git switch -c name	Creates a branch and moves onto it

git merge name	Merges another branch into the current one

git log --oneline --graph	Shows history with branches drawn as a graph

git branch -D name	Force-deletes a branch that wasn’t merged

git check-ignore -v file	Shows which .gitignore rule ignores a file

git fetch --prune	Removes my laptop’s record of branches deleted on GitHub

