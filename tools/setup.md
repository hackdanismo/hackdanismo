# Setup
Here's a guide to setting up development tools on `macOS`.

+ [Terminal](#terminal)
+ [Homebrew](#homebrew)
+ [Git](#git)
+ [NVM](#nvm)
+ [SSH](#ssh)
+ [Bash](#bash)

## Terminal
To close the terminal using the `exit` command:

+ Open the terminal.
+ Press `Cmd + ,` to open the `Settings` panel.
+ Click the `Profiles` tab.
+ Click the `Shell` tab at the top.
+ Find the menu next to `When the shell exits`.
+ Choose `Close the window` or `Close if the shell exited cleanly`.

## Homebrew
Install [Homebrew](https://brew.sh/) from inside the terminal:

```shell
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Once installation has been completed, run these commands in your terminal to add `Homebrew` to your `PATH`:

```shell
$ echo >> /Users/danjackson/.zprofile
$ echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> /Users/danjackson/.zprofile
$ eval "$(/opt/homebrew/bin/brew shellenv zsh)"
```

Close the terminal and reopen to check that `Homebrew` has been installed. The version should be displayed.

```shell
$ brew --version
```

<img width="856" height="81" alt="Terminal showing the Homebrew version following installation." src="https://github.com/user-attachments/assets/8273a4c2-d0cc-4ef4-b10a-30773de07e06" />

Run `$ brew help` in the terminal for any help needed.

## Git
To install `Git` for version control, open the website to view the [installation instructions](https://git-scm.com/install/). For `macOS`, we can use `Homebrew` to install `Git`:

```shell
$ brew install git
```

<img width="857" height="56" alt="Terminal showing git has been installed." src="https://github.com/user-attachments/assets/80d6bceb-b6a4-4471-850c-1e5c1784317e" />

## NVM
Install `Node Version Manager (NVM)` to manage the `Node` version being used.

```shell
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
```

Close and reopen the terminal, or use the following commands:

```shell
$ export NVM_DIR="$HOME/.nvm"
$ [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
$ [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Check the version installed:

```shell
$ nvm --version
```

Install a `Node` version:

```shell
$ nvm install 24.20.0    # Installs Node version 24.20.0
```

Once installed, check the version of `Node` and `NPM` installed:

```shell
$ node --version
$ npm --version
```

<img width="855" height="236" alt="Using the terminal to check the version of Node and NPM installed." src="https://github.com/user-attachments/assets/fad9e08a-f773-43b3-ad99-6635eefc4bc6" />

Within a project file, a `.nvmrc` file can be added to the root of the project folder to specify the version of `Node` to use. This would set the `Node` version to `24.20.0`:

```text
24.20.0 
```

To create a `.nvmrc` file:

```shell
$ node -v > .nvmrc
```

When opening a project, use the following commands in the terminal to install the `Node` version using `NVM` (if not already installed), or set the `Node` version:

```shell
# Read the Node version in the .nvmrc file and install if not already installed
$ nvm install
# Use the Node version specified in the .nvmrc file
$ nvm use
```

## SSH
Setting up `SSH` for `macOS` is useful for working with `GitHub` to pull and push code to repositories. On a new Mac, the cleanest setup is to create a fresh SSH key specifically for the machine, add it to `macOS Keychain`, then register the `public key` with your `GitHub account`. `GitHub` currently recommends `Ed25519 keys`.

Begin by checking that `git` is installed:

```shell
$ git --version
```

Then configure the `name` and `email` Git should put on your commits. Use the email associated with your `GitHub` account:

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "YOUR_GITHUB_EMAIL"
```

Next, generate the `SSH` key using the same email:

```shell
$ ssh-keygen -t ed25519 -C "YOUR_GITHUB_EMAIL"
```

When you see the following, press `ENTER`:

```shell
$ Enter file in which to save the key (/Users/yourname/.ssh/id_ed25519):
```

It is recommended to set a `passphrase` when prompted. `macOS` can remember it in `Keychain`, so you normally won't need to type it repeatedly. We will now have:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

The `.pub` file is safe to give `GitHub`. Never upload or share `id_ed25519` itself.

Next, we need to configure the `SSH` for `macOS`. 

```shell
$ mkdir -p ~/.ssh
$ nano ~/.ssh/config
```

Add this:

```text
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Save in `nano` with `Ctrl+O`, `Enter`, then `Ctrl+X`. `GitHub` recommends this `macOS` configuration so the key is automatically loaded and its passphrase can be stored in Keychain. Now add the key:

```shell
$ ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

Now, copy the `public key` to add it to the clipboard:

```shell
$ pbcopy < ~/.ssh/id_ed25519.pub
```

Once done, Then go to GitHub: `Settings` → `SSH and GPG keys` → `New SSH key` and give the key a name to identify it e.g. `MacBook Pro 2026`. The `Key Type` should be: `Authentication Key`. Paste the `public key` we just copied (`CMS + v`) into the `Key` field.

Click the `Add SSH key` button.

Test the connection:

```shell
$ ssh -T git@github.com
```

If everything has been setup correctly, you should see this message:

```text
Hi hackdanismo! You've successfully authenticated, but GitHub does not provide shell access.
```

`SSH` is set up correctly on this Mac. One final useful check is your Git identity:

```shell
$ git config --global user.name
$ git config --global user.email
```

## Bash

Creating a `bash` script to manage code commits to a `GitHub` repository. This file is named: `git-commit-bash.sh`:

```bash
#!/bin/bash

set -e

echo "----------------------------------------"
echo " Git Commit & Push Assistant "
echo "----------------------------------------"

# Check the script is within a Git repository.
if ! git rev-parse --is-inside-work-tree > /dev/null 2>&1; then
    echo "Error: This directory is not inside a Git repository."
    exit 1
fi

# Show the repository.
REPO_ROOT=$(git rev-parse --show-toplevel)
echo "Repository: $REPO_ROOT"
echo

# The current branch.
CURRENT_BRANCH=$(git branch --show-current)

if [ -z "$CURRENT_BRANCH" ]; then
    echo "Error: You appear to be in a detached HEAD state."
    exit 1
fi

echo "Current branch: $CURRENT_BRANCH"
echo

# Offer to switch branches.
read -r -p "Do you want to switch branches? (y/N): " SWITCH_BRANCH

if [[ "$SWITCH_BRANCH" =~ ^[Yy]$ ]]; then
    echo
    echo "Available local branches:"
    git branch --format='%(refname:short)'
    echo

    read -r -p "Enter the branch name to switch to: " TARGET_BRANCH

    if git show-ref --verify --quiet "refs/heads/$TARGET_BRANCH"; then
        git switch "$TARGET_BRANCH"
        CURRENT_BRANCH="$TARGET_BRANCH"
        echo "Switched to branch: $CURRENT_BRANCH"
    else
        echo "Error: Local branch '$TARGET_BRANCH' does not exist."
        exit 1
    fi 
fi 

echo 
echo "----------------------------------------"
echo " Git Status "
echo "----------------------------------------"

git status --short
echo

# Check whether there are any changes at all.
if git diff --quiet && \
    git diff --cached --quiet && \
    [ -z "$(git ls-files --others --exclude-standard)" ]; then
    echo "Nothing to commit. Working tree is clean."
    exit 0
fi

# Show the full status.
git status
echo

# Ask whether to stage changes.
read -r -p "Stage all changes with 'git add -A'? (Y/n): " STAGE_CHANGES

if [[ ! "$STAGE_CHANGES" =~ ^[Nn]$ ]]; then
    git add -A
    echo "Changes have been staged."
else
    echo
    echo "No changes were staged automatically."
    echo "Only files that have already been staged will be committed."
fi

echo
echo "----------------------------------------"
echo " Staged Changes "
echo "----------------------------------------"

git diff --cached --stat
echo

# Make sure something is staged
if git diff --cached --quiet; then
    echo "Nothing is staged for commit."
    exit 0
fi

# Commit message
read -r -p "Enter a commit message: " COMMIT_MESSAGE

if [ -z "$COMMIT_MESSAGE" ]; then
    echo "Error: The commit message cannot be empty."
    exit 1
fi

echo
echo "Commit message:"
echo " $COMMIT_MESSAGE"
echo

read -r -p "Commit the changes? (Y/n): " CONFIRM_COMMIT

if [[ "$CONFIRM_COMMIT" =~ ^[Nn]$ ]]; then
    echo "Commit has been cancelled."
    exit 0
fi

# Commit
git commit -m "$COMMIT_MESSAGE"

echo
echo "Commit has been created successfully."
echo

# Check the current branch again to confirm it hasn't changed.
CURRENT_BRANCH=$(git branch --show-current)

# Check whether a remote exists.
if ! git remote get-url origin >/dev/null 2>&1; then
    echo "Commit has been completed, but no 'origin' remote is configured."
    echo "Skipping push."
    exit 0
fi

echo "Remote:"
git remote get-url origin
echo

read -r -p "Push '$CURRENT_BRANCH' to origin? (Y/n): " CONFIRM_PUSH

if [[ "$CONFIRM_PUSH" =~ ^[Nn]$ ]]; then
    echo "Commit has been completed. Push skipped."
    exit 0
fi

# Check whether the branch already has an upstream.
if git rev-parse --abbrev-ref --symbolic-full-name '@{u}' >/dev/null 2>&1; then
    git push
else
    echo "No upstream is configured for '$CURRENT_BRANCH'."
    echo "Setting upstream to origin/$CURRENT_BRANCH..."
    git push --set-upstream origin "$CURRENT_BRANCH"
fi

echo
echo "----------------------------------------"
echo " Commit and push completed successfully."
echo " Branch: $CURRENT_BRANCH "
echo "----------------------------------------"
```

Move the file from the `Desktop` to a personal scripts folder, ideally `~/.local/bin`.

```shell
# Creates a folder called .local/bin inside your home directory. The -p means "create parent folders if needed, and don't complain if the folder already exists."
$ mkdir -p ~/.local/bin
# Moves the script from the Desktop into ~/.local/bin. It also renames it from git-commit-bash.sh to git-commit-bash.
$ mv ~/Desktop/git-commit-bash.sh ~/.local/bin/git-commit-bash
# Makes the script executable, so macOS can run it like a command instead of treating it as just a text file.
$ chmod +x ~/.local/bin/git-commit-bash
```

Then make sure that folder is in your `PATH`:

```shell
$ echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
$ source ~/.zshrc
```

After that, from inside any `Git` repository, you can run:

```shell
$ git-commit-bash
```

A couple of useful safeguards are included:

+ it refuses to run outside a Git repo
+ detects a detached HEAD
+ verifies the branch before switching
+ checks that there are actual changes
+ confirms that something is staged before committing
+ automatically sets the upstream branch on the first push.
