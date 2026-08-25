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

## Scripts
A `Python` script usually ends with the file extension of: `.py`. To run the script, for example: `hello.py`:

```shell
$ python3 hello.py
```

The script:

```python
print("Hello, world")
```

## Variables
A `variable` is a name that refers to a `value`.

```python
name = "Dan"
age = 41
is_learning_python = True
```

## Functions
`Functions` are blocks of code that can be reused.

```python
# Function
def say_hello():
    print("Hello, world")

# Call the function
say_hello()
```

A function can also accept `parameters` This example uses an `f-string` to allow values to be added to a `string`.

```python
# Function
def say_hello(name):
    print(f"Hello, {name}")

# Call the function
say_hello("Dan")
```
