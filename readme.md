🦘→📂 **jump** is a lightweight, zero-dependency Zsh module for managing directory aliases. It allows you to bookmark your current directory instantly, jump between projects, and keep your alias list clean and organized.

#### Features

*   **Fast Jump:** Create aliases like `jname` to jump to a directory instantly.
*   **Auto-Cleaning:** The `check` command removes duplicates and invalid paths (directories that no longer exist).
*   **Smart Search:** Typing `j a` shows all aliases starting with `ja`.
*   **Smart Fallback:** If you type `jar` and the alias doesn't exist, it automatically searches for aliases starting with `ar`.
*   **Persistence:** Aliases are saved to `~/.zsh_aliases` and loaded automatically.

#### Installation

##### 1. Install the script

```bash
git clone --depth 1 https://github.com/psqlmaster/jump.git && \
cd jump && sudo cp jump /usr/local/bin && sudo chmod +x /usr/local/bin/jump
```

##### 2. Configure Shell (Safe Append) 
- execute the script for to the add “source /usr/local/bin/jump” to the end of the .bashrc or .zshrc file.
```bash
(
  # Determine which config file to use (prefer .zshrc, fallback to .bashrc)
  RC_FILE="$HOME/.zshrc"
  if [[ ! -f "$RC_FILE" ]]; then
    RC_FILE="$HOME/.bashrc"
  fi
  # Check if the config file actually exists
  if [[ -f "$RC_FILE" ]]; then
    # Check if the line is already present to avoid duplicates
    if ! grep -q "source /usr/local/bin/jump" "$RC_FILE"; then
      echo "" >> "$RC_FILE"
      echo "# jump directory navigation" >> "$RC_FILE"
      echo "source /usr/local/bin/jump" >> "$RC_FILE"
      echo "Success! Added jump to $RC_FILE"
    else
      echo "jump is already installed in $RC_FILE"
    fi
  else
    echo "Error: Could not find .zshrc or .bashrc"
  fi
)
```
---

#### Usage

The main command is `j`.

##### Basic Commands

| Command .     .| Description |
| :--- | :--- |
| **`j a <name>`** | **Add**: Bookmarks the current directory as `j<name>`. |
| **`j r <name>`** | **Remove**: Deletes the alias `j<name>`. |
| **`j l`** | **List**: Shows all saved `j` aliases. |
| **`j c`** | **Check/Clean**: Removes duplicates, sorts the list, and deletes aliases pointing to non-existent folders. |
| **`j h`** | **Help**: Displays the help menu. |

##### Navigation & Search

1.  **Direct Jump:**
    Once an alias is created (e.g., `j a work`), simply type the alias name to jump:
    ```bash
    jwork
    ```

2.  **Smart Search:**
    If you don't remember the exact name, type `j` followed by a search term:
    ```bash
    # Lists all aliases starting with "wo" (jwork, jworld, etc.)
    j wo
    ```

3.  **Auto-Discovery:**
    If you try to run an alias that doesn't exist, `jump` will try to find similar ones:
    ```bash
    # If 'jmissing' doesn't exist, this will list aliases starting with 'missing'
    jmissing
    ```

---

#### Examples

```bash
# 1. Navigate to a project
cd /var/www/html/my-project

# 2. Bookmark it as 'proj'
j a proj
# Output: Added: jproj -> /var/www/html/my-project

# 3. Go somewhere else
cd ~

# 4. Jump back instantly
jproj

# 5. Clean up old aliases (e.g., after deleting project folders)
j c
# Output: Path not found: /tmp/old_test (removing jtest)
# Output: No errors found. File cleaned.
```

