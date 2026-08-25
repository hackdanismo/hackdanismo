# Python

## Install Python
`macOS` comes pre-installed with `Python`. Using the `python` command in the terminal may return: `zsh: command not found: python` meaning that the `python` command is not available. On modern versions of `macOS`, try:

```shell
$ python3 --version

# Install Python using Homebrew:
$ brew install python
```

If `python3` also says `command not found`, install `Python`. The simplest option is usually `Homebrew`. 

Then check:

```shell
$ python3 --version
$ pip3 --version
```

To check where `Python` is installed:

```shell
# May output: /opt/homebrew/bin/python3
$ which python3
```

Update `Python` using `Homebrew`. For example this would update `Python` from `3.14.5` to `3.14.7`:

```shell
$ brew update
$ brew upgrade python
```
