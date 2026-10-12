## Download GitHub files onto empty local folder
```
git clone github_repo_url
```

## Upload local files onto empty GitHub repo
```
git init project_name_goes_here

git add .
git commit -m "commit message goes here"

git remote add origin github_repo_url
git push -u origin main
```

If the git push doesn't work, it's probably because the GitHub repo already has stuff in it. Run these to fix:
```
git pull origin main --allow-unrelated-histories
git push -u origin main
```

Create .gitignore file if there is stuff in your repo that you do NOT want uploaded to GitHub.

## Say who you are
Sometimes git will complain that you haven't identified yourself. Just run:
```
git config user.name "someone"
git config user.email "someone@someplace.com"
```

## Routine push to GitHub
```
git add .
git commit -m "commit message goes here"

git push
```
## Get updates from GitHub
See updates without touching local files:
```
git fetch origin
git log main..origin/main
git diff main origin/main # press q to quit viewer
```

See updates and merge with local files:
```
git pull origin/main
```

## Ignoring Files
Git only tracks files, not folders. Git only knows about folders from the tracked files inside of them.

You can tell Git to NOT keep track of certain files
- create a .gitignore file, and type in rules for which files you want to ignore
- you can filter by file type, or all files in a specific folder
- this prevents re-creatable build files from all being tracked for no reason 
- this can lead to untracked useless files sticking around

### Clean ignored files
Ignoring useless files is good, but it can lead to them sticking around
Delete ignored files with:
```
git clean -ndx    # preview: list what would be deleted
git clean -fdX    # actually delete
```

## Troubleshooting

### refusing to merge unrelated histories
```
git pull origin main --allow-unrelated-histories
```