# Module 2 – Let's Git 🤓

## System Information 
### Note: This is for Linux/Ubuntu 

Distributor ID:	Ubuntu
Description:	Kubuntu-T2 24.04.2 LTS
Release:	24.04
Codename:	noble

=== Current Kernel ===
Linux server000 6.14.0-1-t2-noble 
#1 SMP PREEMPT_DYNAMIC Mon Mar 24 18:22:56 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux


## 1. Setup Git Identity and Credential Manager

```bash
$ git-credential-manager --version # check to ensure it is installed 
$ git config --global user.name "<user name>"
$ git config --global user.email "<user email>"
$ git config --global core.editor "code --wait"
# Set VS Code as the default editor for Git, so whenever Git needs you to write a commit message or edit a rebase, it will open VS Code instead of the default (like nano or vim).
$ git config -l  # list the current git congig
$ git credential-manager github list        # Show list of credential managers
$ git credential-manager github login       # Login as credential manager
```

> 💡 **Note for Linux users:**  
> When running `git credential-manager github list` , I got the following error:
>
> ``` bash
> fatal: No credential store has been selected.
> Set the GCM_CREDENTIAL_STORE environment variable or the credential.credentialStore Git configuration setting to one of the following options:
>
> secretservice : freedesktop.org Secret Service (requires graphical interface)
> gpg           : GNU pass compatible credential storage (requires GPG and pass)
> cache         : Git's in-memory credential cache
> plaintext     : store credentials in plain-text files (UNSECURE)
>
> See https://aka.ms/gcm/credstores for more information.
> ```

To fix this:

```bash
$ git config --global credential.credentialStore secretservice
```

To unset this setting:

```bash
$ git config --global --unset credential.credentialStore
```

Recheck the config:

```bash
$ git config -l
user.name=<github.name>
user.email=<github.login.email.com>
core.editor=code --wait
credential.credentialstore=secretservice
```


> ⚠️ **Authentication Error when running git push**
> Later, when I try run git push (after git add and git commit I got another problem), I got a similar error with a system windows want me to typing the github user and password again:
> ```bash
> $ git push -u origin main
> error: unable to read askpass response from '/usr/bin/ksshaskpass'
> Username for 'https://github.com': abcdef12345
> remote: Invalid username or token. Password authentication is not supported for Git operations.
> fatal: Authentication failed for 'https://github.com/yikai82/test_repo.git/'
> ```

My guess is the authentical issue with git on KDE enviroment and the system is stuck whether to use either HTTP or SSH. JusThis could be due to `ksshaskpass` interfering with Git credentials. To resolve:

```bash
$ git config --global credential.helper manager
```

> This sets Git to use Git Credential Manager (GCM), which requires a credential store backend.
> Based on what I learned from ChatGPT:

🔑 credential.helper=manager

This tells Git to use Git Credential Manager (GCM).GCM is a cross-platform tool maintained by Microsoft.It manages credentials by delegating to a backend credential store, which you must also configure.But by itself, this is not enough — GCM needs a storage backend to save/retrieve your credentials.


🔐 credential.credentialstore=secretservice

This tells GCM what backend to use for storing credentials.In this case:
secretservice is the backend compatible with GNOME/KDE's keyring (like KWallet or GNOME Keyring).Without it, GCM doesn’t know where to store or retrieve your credentials, and will fail.


---

## 2 Create a local git repo and link it with GitHub

Below is an example of creating a local repo call 'test_repo' and make it linked to the github

```bash
$ mkdir test_repo
$ cd test_repo repo
$ git init -b main              # initialize with main as the default branch in your local folder
$ touch test.txt; code test.txt # create test.txt and open in VS code
$ git status
$ git add test.txt
$ git status
$ git log
$ git commit
$ git remote add origin https://github.com/yikai82/test_repo.git
$ git status
$ git remote -v
```
---

## 3. Save (Stash) your local work frist and then upload to the GitHub

Assuming you have make some change in the a working directoy. There were some changes from the GitHub and you are also making some change in your local machine. The strategy is applying change from GitHub first then upload your change to GitHub. 

```bash
$ cd ./path/to/your/work/directory
$ git status                # Check current changes
$ git remote -v             # View origin and upstream paths
$ git stash                 # Save local changes
# Run 'git status' to check your path and the branch 
$ git switch main           # Switch to main branch if the change is going to upload the he main
# change 
$ git switch <branch-name>  #Switch to the branch <branch_name>
$ git pull origin main      # Sync with upstream
$ git stash list            # View saved stashes
$ git stash pop             # Apply stashed changes back
```

---
## 4 Fork a Git Repo from GitHUb and sync with a to Local folder

- The method to fork a Git Repo is very straight folder and can be found here: 
[Link](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#setting-up)

- How to clone it with a local folder, you can choose using VScode or terminal with

```bash
git clone <repository_url>               # Clone an existing repository
git clone <repository_url> <new_dir>     # Clone and rename the directory
```



### ✅ Proper way to switch and track a remote branch:

```bash
$ git fetch origin                # Fetch changes from origin
$ git switch [branch-name]        # Switch to the branch
$ git branch --set-upstream-to=origin/[branch-name]  # Link to remote
$ git push -u origin [branch-name]  # Push and set upstream
```

### After editing:
```bash
$ git add [file-name]       # Stage changes
$ git commit -m "message"   # Commit changes
$ git push                  # Push to GitHub
```
### Other Reference:
1. [U of T DSI github cheatsheet](https://github.com/UofT-DSI/git/blob/main/01_materials/git_cheatsheet.md) 