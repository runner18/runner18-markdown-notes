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

## Troubleshooting

### refusing to merge unrelated histories
```
git pull origin main --allow-unrelated-histories
```