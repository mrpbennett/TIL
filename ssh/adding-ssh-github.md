# Adding a SSH key for Github

First we need to create our SSH key. Use the following, replace `<email>` with
your Github account email address

```bash
ssh-keygen -t ed25519 -C "<email">
```

This will start creating your SSH key. Once done we need to add the SSH key to
our agent. First let's start the SSH key service

```bash
eval "$(ssh-agent -s)"
```

Now the agent is running we need to see if we have our SSH config file run

```bash
~/.ssh/config
```

If you get the following:
`-bash: /home/pbennett/.ssh/config: No such file or directory` this means that
there is no config file present. So lets create one.

```bash
cd

sudo touch ~/.ssh/config
```

The above changes directory to the home directory and creates the file `config`
in `.ssh` now we need to edit our config, open up Vim.

```bash
vim ~/.ssh.config
```

Then add the following:

```bash
Host *
    AddKeysToAgent yes
    IdentityFile ~/.ssh/id_ed25519
```

Now we can add the key to our agent like so:

```bash
ssh-add ~/.ssh/id_ed25519
```

This will go ahead and add your SSH key to the agent.
