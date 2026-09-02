# Setup
Here's a guide to setting up development tools on `macOS`.

+ [Terminal](#terminal)
+ [Homebrew](#homebrew)
+ [Git](#git)
+ [NVM](#nvm)

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

