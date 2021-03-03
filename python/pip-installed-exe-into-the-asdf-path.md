# Get pip installed executables into the asdf path

Given I am using `asdf` when I install a python executable via `pip`, then I expect to be able to run that executable at the command line or find it's location using `which`

```zsh
> asdf global python 3.7.2
> pip install some-executable-package
> some-executable
zsh: command not found: some-executable
```

OR

```zsh
> which flake8
zsh: flake8 not found
```

It doesn’t exist.

To place a shim into the shims dir of asdf you must `reshim`.

```zsh
> asdf reshim python
```
And now you have a shim that points to the executable:

```zsh
> which flake8
zsh: /Users/paul/.asdf/shims/flake8
```
