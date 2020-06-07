# Pipenv Usage

Getting virtual enviroments and keeping packages all in ship shape I found hard. That's before I found `pipenv` which to me is similar to NPM and `package.json`

### Example Pipenv Workflow.

Clone / create project repository:
`$ cd myproject`

Install from Pipfile, if there is one:
`$ pipenv install`

Or, add a package to your new project:
`$ pipenv install <package>`

This will create a Pipfile if one doesn’t exist. If one does exist, it will automatically be edited with the new package you provided.

Next, activate the Pipenv shell:
```
$ pipenv shell
$ python --version
```

This will spawn a new shell subprocess, which can be deactivated by using `exit`.

#### Specifying Versions of a Package

```python
$ pipenv install requests~=1.2
```

Pipenv will install version 1.2 and any minor update, but not 2.0.

This will update your Pipfile to reflect this requirement, automatically