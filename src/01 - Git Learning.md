# Starting off with Git : Background, Configuration, Installation, Application
[Git-SCM](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)  
[Git-SCM Primary Documentation](https://git-scm.com/doc)
## GitHub Sub-Repo // Full List of Info Gathered

 Git Preliminary / Initial Notes 

- Git has it's own software that needs to be installed
	- GitHub for Desktop
	- VS Code
	- Git
- Git to GitProvider / Cloud
- File Explorer > Cloud > Git
- File & File Recipient create a mutual pool
- Working on the same file at same time: 
- Normally two files can merge into one w/ built in code.
- Reported features can appear for Open Source assistance 

- ***GitProvider Features***
	
	- Branch: Direct copy from the repo to allow multi-use on a file.
	- You can clone a repo but branches cannot be part of more than one repo.
	- Branching allows multiple copies to go in tandem at the same time.
	- If multiple users & multiple places are working on a document at the same time:
		- More issues are likely to occur.

## LinkedIn Learning - Git Essentials Course #1

### LinkedIn Learning - GitEssentials

---

- Git has it's own software that needs to be installed. 
- Git to GitProvider / Cloud
Essential Baseline of "what is GIT":
File Explorer > Cloud > Git
Git Share Methods
- Share Code via GitProvider (GitHub / Go PUBLIC or Private
File to Git Provider to File Recipient 
- File & File Recipient create a mutual pool
- Working on the same file at same time: 
	- Normally two files can merge into one w/ built in code.
- "Report" / flag features can appear for Open Source assistance to improve communication
- See branching for multiple people working on the same source. Decreases likelihood of changes not being tracked. 
GitProvider Features
- Branch: Direct copy from the repo to allow multi-use on a file.
	- You can clone a repo but branches cannot be part of more than one repo.
- Branching allows multiple copies to go in tandem at the same time.
- Think of OneDrive & how sync issues occur for users who work on the document locally while users stay on the live / real-time web version.

---

### Git Essential Training | GitHub + Git Providers 
#### How Does Git Work? What is Git? Why Use Git?
- Source provider (i.e. Coder) uploads via GitProvider Application
- See notes: Branching / Sharing Options via Public+Private Links
- Providers such as GitHub
- Install Locally:
- Example:
	- Remote Repository @ https://github.com/GitRepo
	- Local PC = Local repository location (.git)
	- Staging Area > Local Staging File Structure (C:\Scripts\GitRepo\)
- From Local PC CLI - Run something such as:  _
- Git Pull
- Git Push 
- Git Add .path
- Git = Distributed Version Control 
- OneDrive/DropBox = Centralized Version Control
- If I change a file on my PC then the server updates
- If something happens to the server, then I go back to my last file copy.
	- In Distributed Version - The files are centrally located to everybody, therefore everybody has access & if the GitProvider goes down, then all with access still have local versions of all copies. 
- ***No one understands everything about Git***

### Install & Configure Git
#### [Install Git - Windows](https://git-scm.com/download/win)
+ Set to VSCode & Install VSCode Extensions > GitHistory,GitLens,GitGraph

Configuring Git:
- Gitconfig
- Global Config : C:\Users\User.gitconfig
- Local = .git/config
- Default editor / branch / aliases

| Commands   | Data Information   | Extended Info  |
| ------------------|:----------|:----------|
| Git Init  | Initialize / activate folder path / GitHub repo| |
| Git remote add | Defines the remote repository & then connects the two | |
| Git Add      | Stage the changes  | - |
| Git Commit    | Send staged to local repo (+commit message) | - |
| Git Push | Push committed changes to remote repository | The files must be staged & committed before being pushed. Only sends changes to the remote repository that are in the Local Repository as well. |
| Git Pull |  Adds the file to a local repo & directly to the local folder | - |
| Git config --list |  Git config --global user.name "my Name" | Git config --global user.email "myName@email.com" |
| Git status | Check ^HEADER & status of repo/sync ||
| Git diff | shows what has been changed local vs remote | Git diff --cached before you make a commit |
| Git log --grep='expression' | Git log -p | Git log --graph |
| Git show [checksum] | 
| Example Removal of File Sequence: | Git add .  then Git push | Git commit -m 'Removed example' |
```
Even without internet you can still go back to earlier versions, make snapshots as you go, and revert to changes made earlier in the script
```
Corrupting the Main Repo: 
+ Git is distributed, the file in users repositories will be unaffected by any changes to the file in the main repository unless users attempt to retrieve the corrupted file" 
+ If a user corrupts the Main Repo > They can revert to a previous version & then advise people to retrieve the new copy.


- Renaming Note: Git won't understand at first it is a new file. After committing the change, it will merge.
- Git mv .\example.txt example-renamed.txt (Rename)
Folders:
- Git ignores empty folders
- .gitkeep (Important to structure, but no data to keep)
Revert to Previous Commit / Undoing Commits:
- If : git add .
- Let's revert? Git status
- Git restore --staged <file…>
- Git restore --staged .
- Go further back / delete these chages?
- Git restore .
- Git log 
- Git checkout (paste commit unique ID from git log) 
- Git checkout main (return statement)  
Revert to the Previous Version:  
- Git log --oneline (prints out in one line)
- Git revert [commit ID to revert to]
- History is always added. Git revert adds a new source line for 'git commit reverted'

Create file called importantfile.md
- Create folder
- Add file into folder
- Git init > make repository 
- Git commit -m 'initial commmit'
- Git add .
- Git commit -m 'add line'
- Git add . 
- Git log --oneline

### File Structure: .gitIgnore , .md , .yml
- .gitignore 
- Filelog1.txt in some repository
- If you add filelog1.txt to the .gitignore file then git will continue to sync past this
- Logfiles/*.md ignores all .md in the directory but you can drill down on this
#### Git Branches
- Branch = copy of code / backup / version history
- Main branch keeps copy of everything in commit history
- Working an issue or working on a parallel branch from a fork & then when ready to **merge**
	- Create pool request
	- Merge
	- Delete old branch & have it merge into the file structure of main

#### Git Commit Messages (Git commit -m '\<msg>')
```cmd
Present tense "fixes [task]" 
~50 char
Create meaningful messages. 
Git commit -m 
Opens text editor / VS Studio Code
Message can be multiple lines.
```
#### Creating Branches
```
Git branch main.GitLearning
Git checkout main.GitLearning or Git switch main.
GitLearning
Git status (confirms which branch you are in) / the HEAD will point here.
- Renaming = git branch -m [new name]
- Publish / upload what you have locally to the remote server (PUBLIC SHARE TO THE REMOTE SERVER)
```

Local branch 'develop' - local pc 
Remote branch 'origin/develop' - tracking
Git branch --track [pathTo] [pathFrom]
- git branch --track main main.PowerShell
- **REMINDER** - You must publish the local branch to make it present on the timeline.

- **Git checkout --track [pathFrom] [pathTo]**
	- Brings Remote to Local (Branch : Branch) 
- it push -u (Public)
- Git checkout --track
- Git pull (download from remote) 
- Git push (upload to remote)

#### DELETE LOCAL CHANGES
- Git branch -v
- Git branch -d -f main.PowerShell
#### DELETE REMOTE CHANGES 
Git push origin --delete main.\<branchname>
#### Merging
1. Switch to branch receiving changes
2. Run merge 
Git switch main 
Git merge main.PowerShell

#### Diffing 
- Red Triangle shows diffs in VS Code
- Git Diff	        • Top right has a side-by-side show changes view
- Source Control Panel
- Clicking on a file that has changed will give you the diff view
- Select for Compare
- Also: View Timeline in VS Code
- VS Code Logs - Output Log	
	- Looking to up your game?	Copilot = Suggestion based VS Code for git
	- Fork a repository & then drift through the logs for a revert point.
- Git log --pretty=oneline
- Git log --pretty=format:"%H - %an - %ar : %s"
