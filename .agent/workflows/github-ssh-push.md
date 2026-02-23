---
description: Push repository to GitHub using SSH credentials
---
// turbo-all

This workflow ensures that the GitHub repository uses an SSH remote URL (preventing HTTPS password prompts in headless environments) and then pushes the current branch.

1. Automatically convert the `origin` remote from HTTPS to SSH if needed:
`git remote -v | awk '/^origin.*\(fetch\)/ {url=$2; if (url ~ /^https:\/\/github\.com\//) { sub(/^https:\/\/github\.com\//, "git@github.com:", url); cmd="git remote set-url origin " url; system(cmd); print "Changed origin to " url } else { print "Origin is already SSH or not GitHub" } }'`

2. Push to the remote tracking branch:
`git push`
