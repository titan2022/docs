# Git

This is a short cheatsheet on VSCode source control usage. Feel free to use Git CLI if you already have experience or want to learn it. Full guide on VSCode source control can be found [here](https://code.visualstudio.com/docs/sourcecontrol/overview).

## Structure

* A Git project is called a repository
* Github is a Git platform for hosting Git projects online
* Inside a repo, you can create branches
* Typically, the `main` or `master` is used in production
* Each branch contains a list of changes called commits
* Each commit contains changes to files and a message
* All commits are created locally; you need to do a push to upload them to Github
* To download changes, do a pull
* To update your list of branches, do a fetch (VSCode has an option to do this automatically)
* You can dublicate branches, work on them separately, and merge them back together
* This allows everyone to work on different features independently without breaking things

## Cloning

* Cloning is downloading a repository
* First, click on green Code button and copy the HTTPS link

![Copying HTTPS link](./git_clone.png "Copying HTTPS link")

* Second, go to VSCode source control tab and click clone

![Cloning](./vsc_clone.png "Cloning")

* Paste your link and press enter
* Select a folder to save the project in

![Pasting](./vsc_paste.png "Pasting")

* Open the same folder next time to open the project
* Please don't clone the repo every time you want to work on it!

## Committing

* First, make the changes you want to make
* Go to source control tab and press `+` for all files you want to include in your commit
* Write a message (a description) for your commit (this step is required)
* You can press `-` for files you want to remove from your commit
* Once you are ready, press commit button

## Pushing & Pulling

* After committing, you need to push
* You can click VSCode's blue "sync" button, which will push and pull at the same time
* Otherwise, you can click on additional actions and click push or pull

![Push/pull actions](./vsc_push_pull.png "Push/pull actions")

## Switching branches

* To check which branch you are on, view the message input bar 

![Checking branch](./vsc_branch.png "Checking branch")

## Merging

* 