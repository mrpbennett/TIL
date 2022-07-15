# Using TOML with Python

[Python and TOML: New Best Friends](https://realpython.com/python-toml/)

TOML—Tom’s Obvious Minimal Language—is a reasonably new configuration file format that the Python community has embraced over the last couple of years. TOML plays an essential part in the Python ecosystem. Many of your favorite tools rely on TOML for configuration.

### config.toml

```toml
[user]
player_x.color = "blue"
player_o.color = "green"

[constant]
board_size = 3

[server]
url = "https://tictactoe.example.com"
```

### Accessing the config in Python code

In this section, you’ll work with a relatively new package called [tomli](https://pypi.org/project/tomli/) and its sibling [tomllib](https://docs.python.org/3.11/library/tomllib.html). These are great libraries when you only want to load a TOML document into Python

```python
# You first open the file, using a context manager to handle any issues that may show up. 

with open("config.toml", mode="rb") as config_file:
    config = tomli.load(config_file)


print(config["user"]["player_o"]) # prints out green
```