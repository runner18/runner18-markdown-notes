How to have a VS Code workspace inside a DevContainer, all inside WSL
- WSL
	- DevContainer
		- VS Code workspace

Install
1. VS Code Extensions:
	1. Dev Container
	2. WSL
2. Docker Desktop
	1. Must open Docker Desktop to ensure Docker engine is running
	2. Enable WSL integration

Open repo, then run these commands (ctrl + shift + P):

1. WSL: Reopen Folder in WSL
2. Dev Containers: Reopen in Container
	1. First time, will take some time to download Dev Container
3. Dev Containers: Open Workspace in Container...
	1. Select the workspace file
	2. It may pop up a warning, click 'Continue'

You might be able to skip step 2, idk