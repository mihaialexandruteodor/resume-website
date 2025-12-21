---
title:  Multi-User SSH Key Setup
date: 2025-12-15
categories: [GIT]
tags: [git, quality-of-life]
---

This tutorial guides you through creating two distinct SSH keys (Personal and Work) and configuring your local SSH client (`~/.ssh/config`) to automatically use the correct key for different remote users or projects.

## Prerequisites

* You are running Arch Linux or any Linux distribution with `ssh-keygen` installed.
* You have access to a terminal.

## Step 1: Generate Separate SSH Key Pairs

We will use the modern, secure **Ed25519** algorithm and save the keys with distinct names in your default `~/.ssh/` directory.

### Key 1: Personal Key

```zsh
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_personal -C "your_personal_email@example.com"
```

### Key 2: Work Key

```zsh
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_work -C "your_work_email@company.com"
```

> You will be prompted to enter a **passphrase** for each key. It is highly recommended to use one for security.
{: .prompt-info }

### Verify Key Permissions

SSH requires strict permissions on private keys. Ensure they are set correctly:

```zsh
chmod 600 ~/.ssh/id_ed25519_personal ~/.ssh/id_ed25519_work
ls -l ~/.ssh/
```

You should see files like:
* `id_ed25519_personal` (Private)
* `id_ed25519_personal.pub` (Public)
* `id_ed25519_work` (Private)
* `id_ed25519_work.pub` (Public)

## Step 2: Configure the SSH Client (`~/.ssh/config`)

Create or edit your SSH configuration file to define rules for when each key should be used. This is done by creating **Host Aliases**.

```zsh
nano ~/.ssh/config 
# Or use vim, micro, etc.
```

Paste the following structure into the file. **You must replace the example `HostName` and `IdentityFile` paths if you named your keys differently.**

```ssh
# ------------------------------------------------------------------
# PERSONAL ACCOUNT CONFIGURATION (Example: Personal GitHub)
# ------------------------------------------------------------------
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes
    # Optional: Automatically add key to the agent after first use
    # AddKeysToAgent yes

# ------------------------------------------------------------------
# WORK ACCOUNT CONFIGURATION (Example: Work GitHub)
# ------------------------------------------------------------------
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes

# ------------------------------------------------------------------
# WORK ACCOUNT CONFIGURATION (Example: Work GitLab)
# ------------------------------------------------------------------
Host gitlab-work
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes

# ------------------------------------------------------------------
# GENERIC/DEFAULT SETTINGS (Applies to all connections unless overridden)
# ------------------------------------------------------------------
Host *
    ForwardAgent yes
    # Set default connection settings here
```

**Key Directives Explained:**
* **`Host`**: The custom alias (e.g., `github-personal`) you will use in your commands.
* **`HostName`**: The actual server address (e.g., `github.com`).
* **`IdentityFile`**: **Crucial** path to the private key for this specific host.
* **`IdentitiesOnly yes`**: Tells SSH to *only* use the specified key for this connection, preventing authentication issues.

## Step 3: Add Public Keys to Remote Services

The private key stays on your Arch machine; the corresponding public key (`.pub` file) must be uploaded to the remote services (GitHub, GitLab, etc.).

1.  **Copy Personal Public Key:**
    ```zsh
    cat ~/.ssh/id_ed25519_personal.pub
    ```
    *Copy the entire output and paste it into the **personal** account's SSH key settings.*

2.  **Copy Work Public Key:**
    ```zsh
    cat ~/.ssh/id_ed25519_work.pub
    ```
    *Copy the entire output and paste it into the **work** account's SSH key settings.*

## Step 4: Use the Correct Key in Your Folders (Git Usage)

To ensure different folders use the correct keys, you must replace the standard hostname with your configured **Host Alias** when cloning or setting the remote URL.

### Scenario A: Personal Repository

| Standard Clone URL | Alias Clone URL (Correct) |
| :--- | :--- |
| `git@github.com:personal-user/repo.git` | `git clone git@github-personal:personal-user/repo.git` |

### Scenario B: Work Repository

| Standard Clone URL | Alias Clone URL (Correct) |
| :--- | :--- |
| `git@github.com:work-org/repo.git` | `git clone git@github-work:work-org/repo.git` |

Once you clone the repository using the alias, Git stores that alias in the repository's configuration, meaning all future push/pull operations from within that folder will automatically use the correct private key defined in your `~/.ssh/config`.

### Test the Connection

You can verify that the correct key is being used by testing the connection:

```zsh
# Test connection using the personal key
ssh -T git@github-personal

# Test connection using the work key
ssh -T git@github-work
```

> Your Git username and email won't differ by default, even if you used different SSH keys to clone or commit to the repos. Git's authentication (SSH Key) and identity (User Name/Email) are entirely separate.
{: .prompt-warning }

#  Automating Git Identity with Conditional Includes

This method ensures that your Git `user.name` and `user.email` automatically switch based on the project's directory path. This is the **recommended solution** for managing multiple identities simultaneously. 

## Step 1: Set Your Default (Personal) Identity

Edit your global Git configuration file (`~/.gitconfig`) to define your most common identity (e.g., your personal one).

```bash
nano ~/.gitconfig
# Use your preferred text editor (vim, micro, etc.)
```

### `~/.gitconfig` Content (Default Identity & Conditional Include)

```ini
[user]
    name = Personal Name
    email = personal@example.com

# --- Conditional Include ---
# This checks if the repository's path matches a pattern.
# We use a placeholder for the "Work" directory. Replace '~/Work/' with your actual path!
# The '/**' (double asterisk) ensures it matches ALL nested repositories inside the 'Work' directory.
[includeIf "gitdir:~/Work/**"]
    path = ~/.gitconfig-work
```

## Step 2: Create the Override Configuration File

Create a separate configuration file (`~/.gitconfig-work`) containing only the identity settings for your secondary projects.

```zsh
nano ~/.gitconfig-work
```

### `~/.gitconfig-work` Content (Work Identity Override)

```zsh
[user]
    name = Work Name
    email = work@company.com
```
## Step 3: Check the username and/or email at repo level

```zsh
git config user.name
git config user.email
```

### How It Works

1.  **Default Behavior:** If you are *not* inside the path specified by the `includeIf` condition, Git uses the `[user]` section from `~/.gitconfig` (Personal Identity).
2.  **Conditional Override:** When you navigate inside a repository located under the `~/Work/` path, the `[includeIf]` condition is met. Git then loads `~/.gitconfig-work`, and its `[user]` section **overrides** the default one for that specific directory, setting your Work Identity for all commits.