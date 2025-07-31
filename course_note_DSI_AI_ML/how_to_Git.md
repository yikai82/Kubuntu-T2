# Module 2 – Let's Git 🤓

## System Information 
### Note: This is for Linux/Ubuntu 

Distributor ID:	Ubuntu  
Description:	Kubuntu-T2 24.04.2 LTS  
Release:	24.04  
Codename:	noble  

**=== Current Kernel ===**  
Linux server000 6.14.0-1-t2-noble  
#1 SMP PREEMPT_DYNAMIC Mon Mar 24 18:22:56 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

---

## 1. Setup Git Identity and Credential Manager

```bash
$ git-credential-manager --version   # Check to ensure it is installed 
$ git config --global user.name "<user name>"
$ git config --global user.email "<user email>"
$ git config --global core.editor "code --wait"
# Set VS Code as the default editor for Git, so whenever Git needs you to write a commit message or edit a rebase, it will open VS Code instead of the default (like nano or vim).
$ git config -l  # List the current Git config
$ git credential-manager github list        # Show list of credential managers
$ git credential-manager github login       # Login as credential manager
```

> 💡 **Note for Linux users:**  
> When running `git credential-manager github list`, you might see the following error:
>
> ```bash
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

> ⚠️ **Authentication Error when running `git push`:**  
> Later, when I tried to run `git push` (after `git add` and `git commit`), I got another issue — a system window popped up asking for GitHub credentials:
>
> ```bash
> $ git push -u origin main
> error: unable to read askpass response from '/usr/bin/ksshaskpass'
> Username for 'https://github.com': abcdef12345
> remote: Invalid username or token. Password authentication is not supported for Git operations.
> fatal: Authentication failed for 'https://github.com/yikai82/test_repo.git/'
> ```

My guess is that this is a Git authentication issue with the KDE environment — possibly caused by `ksshaskpass` conflicting with Git credential handling.

To resolve:

```bash
$ git config --global credential.helper manager
```

> This tells Git to use Git Credential Manager (GCM), a cross-platform tool maintained by Microsoft.  
> It manages credentials by delegating to a backend credential store, which must also be configured.

### 🔑 Credential Summary:

- `credential.helper=manager`  
  Uses Git Credential Manager (GCM). Required for managing login sessions with GitHub or other providers.

- `credential.credentialStore=secretservice`  
  Specifies the backend GCM uses for storing credentials (compatible with GNOME Keyring / KDE Wallet).

---

## 2. Create a Local Git Repo and Link It to GitHub

Below is an example of creating a local repo called `test_repo` and linking it to GitHub:

```bash
$ mkdir test_repo
$ cd test_repo
$ git init -b main              # Initialize with main as the default branch
$ touch test.txt; code test.txt # Create test.txt and open it in VS Code
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

## 3. Save (Stash) Local Work First, Then Sync with GitHub

If you’ve made changes in a local working directory and want to pull updates from GitHub, the strategy is:

**1. Stash local work**  
**2. Pull from GitHub**  
**3. Reapply local changes**

```bash
$ cd ./path/to/your/work/directory
$ git status                # Check current changes
$ git remote -v             # View origin and upstream paths
$ git stash                 # Save local changes
# Check your current branch
$ git switch main           # Switch to main branch if pushing to main
$ git switch <branch-name>  # Switch to target branch, if not main
$ git pull origin main      # Pull changes from GitHub
$ git stash list            # View saved stashes
$ git stash pop             # Reapply stashed changes
```

---

## 4. Fork a Git Repo from GitHub and Sync to a Local Folder

- To fork a Git repo, follow this guide:  
  👉 [UofT DSI Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#setting-up)

- To clone your forked repo into a local folder:

```bash
$ git clone <repository_url>               # Clone an existing repository
$ git clone <repository_url> <new_dir>     # Clone and rename the directory
```

---

### ✅ Proper Way to Switch and Track a Remote Branch

```bash
$ git fetch origin
$ git switch [branch-name]
$ git branch --set-upstream-to=origin/[branch-name]
$ git push -u origin [branch-name]
```

### After Editing:

```bash
$ git add [file-name]        # Stage changes
$ git commit -m "message"    # Commit changes
$ git push                   # Push to GitHub
```

---

### 📚 Other References:
1. [U of T DSI GitHub Cheatsheet](https://github.com/UofT-DSI/git/blob/main/01_materials/git_cheatsheet.md)
