# First time on danserver

For starting on the server make sure to read:

- [DanServer manual](https://sgn102.pages.ku.dk/a-not-long-tour-of-dangpu/)
- [Genomics Platform wiki](https://sundgenomics.github.io)
- **platforms**: [JupyterHub](http://dangpu01fl:8989) and [RStudio](http://dangpu01fl:8787)

## First time on server checklist

1. Login to danhead: `ssh $USER@danhead01fl.unicph.domain`
2. Run `nano ~/.bash_profile`

```text
if [ -f ~/.bashrc ]; then
 . ~/.bashrc
fi
```

3. Run `nano ~/.bashrc`

```text
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
 . /etc/bashrc
fi

# User specific environment
if ! [[ "$PATH" =~ "$HOME/.local/bin:$HOME/bin:" ]]
then
    PATH="$HOME/.local/bin:$HOME/bin:$PATH"
fi
export PATH

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
### Source DanGPU definitions
if [ -f /maps/projects/dan1/apps/etc/bashrc ]; then
 . /maps/projects/dan1/apps/etc/bashrc
fi

### Source Brickman definitions
if [ -f /maps/projects/dan1/data/Brickman/config/brickman.bashrc ]; then
 . /maps/projects/dan1/data/Brickman/config/brickman.bashrc
fi
```

4. Logout and login again, you should have see now `Brickman` folder
