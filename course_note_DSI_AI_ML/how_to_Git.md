## Module 2 – Git

### Note & System Information
Distributor ID:	Ubuntu
Description:	Kubuntu-T2 24.04.2 LTS
Release:	24.04
Codename:	noble

=== Current Kernel ===
Linux server000 6.14.0-1-t2-noble #1 SMP PREEMPT_DYNAMIC Mon Mar 24 18:22:56 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux


### 1. Setup Git Identity and Credential Manager

```bash
$ git-credential-manager --version # check if you have git installed 
$ git config --global user.name "<user name>"
$ git config --global user.email "<user email>"
$ git config --global core.editor "code --wait"
$ git config -l  # list the current git congig
$ git credential-manager github list        # Show list of credential managers
$ git credential-manager github login       # Login as credential manager
```

> 💡 **Note for Linux users:**  
> Running `git credential-manager github list` may result in the following error:
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

Recheck your config:

```bash
$ git config -l
user.name=yikai82
user.email=yikai.su89@gmail.com
core.editor=code --wait
credential.credentialstore=secretservice
```


> ⚠️ **Authentication Error when running git push**
> Later, when I try run git push (after git add and git commit I got another problem):
> ```bash
> $ git push -u origin main
> error: unable to read askpass response from '/usr/bin/ksshaskpass'
> Username for 'https://github.com': yikai82
> remote: Invalid username or token. Password authentication is not supported for Git operations.
> fatal: Authentication failed for 'https://github.com/yikai82/test_repo.git/'
> ```

This could be due to `ksshaskpass` interfering with Git credentials. To resolve:

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

### 3.2 Create a Local Git Repo and Link to GitHub
Below is a example of geenrate a local repo call 'test_repo' and make it linked to the github


```bash
$ mkdir test_repo
$ cd test_repo repo
$ git init -b main
$ touch test.txt; code test.txt
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

### 3.3 Save Local Work and Sync with Upstream

```bash
$ cd ./path/to/your/work/directory
$ git status         # Check current changes
$ git remote -v      # View origin and upstream paths
$ git stash          # Save local changes
$ git switch main        # Switch to main branch
$ git switch assignment  # Or switch to assignment branch
$ git pull origin main   # Sync with upstream
$ git stash list         # View saved stashes
$ git stash pop          # Apply stashed changes back
```

---

### 3.4 Fork a Git Repo and Download to Local

1. Fork the `shell` repo from DSI GitHub to your GitHub.
2. Create a branch called `assignment` on GitHub.

Then in terminal:

```bash
$ git status    # Expected error: not a git repository
$ cd /path/to/the/repository
$ git status
$ git checkout -b assignment     # Create and switch to new branch
$ git checkout assignment        # Switch to existing branch (if already created)
```

#### ✅ Proper way to switch and track a remote branch:

```bash
$ git fetch origin                # Fetch changes from origin
$ git switch [branch-name]        # Switch to the branch
$ git branch --set-upstream-to=origin/[branch-name]  # Link to remote
$ git push -u origin [branch-name]  # Push and set upstream
```

#### After editing:
```bash
$ git add [file-name]       # Stage changes
$ git commit -m "message"   # Commit changes
$ git push                  # Push to GitHub
```
