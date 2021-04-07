# Installing Conda and using pandas on M1 macs

Install Anaconda via home brew

```zsh
brew install anaconda
```

then set your path in your `.zsh` file.

```zsh
~/../../opt/homebrew/anaconda3/bin/conda init zsh
```
This will place the following in your zsh file.

```zsh
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/opt/homebrew/anaconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/opt/homebrew/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/opt/homebrew/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/opt/homebrew/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
``` 

which will you to get your executable path too, for example `'/opt/homebrew/anaconda3/bin/conda`

### Using Pandas

Just simply run `conda install pandas`
