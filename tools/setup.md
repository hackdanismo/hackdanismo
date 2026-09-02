# Setup
Here's a guide to setting up development tools on `macOS`.

+ [Terminal](#terminal)
+ [Homebrew](#homebrew)
+ [Git](#git)
+ [NVM](#nvm)
+ [SSH](#ssh)

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
