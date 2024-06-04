# 011 Improve Your Terminal (Easy Way)

Tested on Windows Subsystem For Linux (WSL):
- **OS Name:** Microsoft Windows 11 Home
- **OS Version:** 10.0.22631 Build 22631
- **WSL Version:** 2.1.5.0
- **WSL Distribution:** Ubuntu-22.04

This should work on other Linux distributions and macOS with eventual tweaks.

## Commands ⌨️

### Install zsh
Source [here](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH).

```bash
sudo apt install zsh
```

### Install ohmyzsh
Source [here](https://github.com/ohmyzsh/ohmyzsh).

```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Check current default shell

```bash
echo $SHELL
```

### Make BASH the default shell

```bash
chsh -s $(which bash)
```

### Make zsh the default shell

```bash
chsh -s $(which zsh)
```

### Check ssh connection to GitHub
Source [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection).

```bash
ssh -T git@github.com
```

### Add the following lines to .zshrc file to automate ssh passphrases
Source [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases).

```bash
# Auto-lauching ssh-agent
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```
