---
title: Search ZSH aliases
date: 2025-12-21
categories: [LINUX, TIPS]
tags: [linux, quality-of-life, shell]     # TAG names should always be lowercase
---

# LINUX QUALITY OF LIFE IMPROVEMENTS #2

This nice function will search all the aliases in your `~/.zshrc` config.

> Your alias must be preceded by a comment, a line starting with the `#` character, for it to be displayed in the output
{: .prompt-warning }

Example:
```zsh
# Update the whole system (Nobara way)  <-- this line will show in the Alias column
alias update='sudo nobara-sync cli'
```

### Step 1. open .zshrc with nano, nvim or other editor

```zsh
nvim ~/.zshrc
```

### Step 2. Add the function

```zsh
# Function to list aliases and their descriptions only
show-aliases() {
    echo -e "\033[1;32mDescription\033[0m                              \033[1;36mAlias\033[0m"
    echo "------------------------------------------------------------"
    
    grep -B 1 "^alias " ~/.zshrc | \
    sed 's/--//g' | \
    awk '
    # Store the comment line
    /^#/ { comment = $0; next }
    
    # Process the alias line
    /^alias / {
        sub(/^alias /, "", $0);          # Remove "alias " prefix
        split($0, parts, "=");           # Split at =
        name = parts[1];                 # Take only the shortcut name
        
        # Print Comment (Gray) and Alias (Cyan)
        printf "\033[0;90m%-40s\033[0m \033[1;36m%s\033[0m\n", comment, name;
        
        comment = ""; # Reset for next
    }'
}
```

### Step 3. Reload ZSH

```zsh
source ~/.zshrc
```
### Step 4. Test show-aliases

```zsh
show-aliases
```

Output will look like this:

```zsh
Description                              Alias
------------------------------------------------------------
# iPad screen mirroring                  ipad-mirror
# Update the whole system (Nobara way)   update

```