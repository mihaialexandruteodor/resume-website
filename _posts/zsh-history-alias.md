---
title: Alias to search ZSH history
date: 2025-12-13 20:23:00
categories: [LINUX, TIPS]
tags: [linux, quality-of-life, shell]     # TAG names should always be lowercase
---

# LINUX QUALITY OF LIFE IMPROVEMENTS #1

## Alias to search ZSH history
edit '~/.zshrc' and add
```
# Function to search history using grep
search () {
  # The "$@" variable passes all arguments received by the function to grep
  history 1 | grep --color=always "$@"
}
```

then reload the shell
```
source ~/.zshrc
```
## 🏷️ `search` Function Usage

| Command                      | Description                                                                                                                                    | Example Output (Hypothetical)                    |
| :--------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------- |
| `search <term>`              | Searches your entire history for any line containing the specified term.                                                                       | `2017  git config --global user.name "John Doe"` |
| `search "<phrase>"`          | Searches for an exact phrase (useful for commands with spaces). **Note the quotes.**                                                           | `345  docker run -it ubuntu bash`                |
| `search <term1> <term2>`     | Searches history for lines containing **both** `term1` AND `term2`.                                                                            | `123  sudo apt update && sudo apt upgrade`       |
| `search -i <term>`           | Performs a **case-insensitive** search (passes `-i` to `grep`).                                                                                | `456  git status`                                |
| `search -E "(term1\|term2)"` | Uses extended regex to search for lines containing **either** `term1` OR `term2`. (Backslash escapes the pipe `\|` to prevent table breakage). | `789  ls -l`                                     |

