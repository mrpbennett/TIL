# Prevent Conda from activating the base environment by default?

After install Anaconda or Miniconda it sets your enviroment byt default. This is
sometimes quite annoying. To disable that behaviour run this from your terminal

```bash
conda config --set auto_activate_base false
```

The first time you run it, it'll create a `.condarc` in your home directory with
that setting to override the default.

This wouldn't de-clutter your `.bashrc` or `.zshrc` files but it's a cleaner
solution without manual editing that section that conda manages.

### Activate / Deacticate conda env

Then once this is in place you can simply use the following to activate /
deactivate your conda env.

```bash
conda activate <env>
```

```bash
conda deactivate
```
