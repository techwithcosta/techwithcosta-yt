# 010 Awesome BASH Trick (For Git Users)

Tested on Windows Subsystem For Linux (WSL):
- **OS Name:** Microsoft Windows 11 Home
- **OS Version:** 10.0.22631 Build 22631
- **WSL Version:** 2.1.5.0
- **WSL Distribution:** Ubuntu-22.04

This should work on other Linux distributions and macOS with eventual tweaks.

## Commands ⌨️

### Backup .bashrc file

```bash
cp .bashrc .bashrc.bak
```

### Delete the following lines from .bashrc

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

### Replace those lines with the following on .bashrc

```bash
# Add git branch to prompt
parse_git_branch() {
    git rev-parse --is-inside-work-tree >/dev/null 2>&1 || return
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/' | sed 's/^/ /'
}
if [ "$color_prompt" = yes ]; then
 PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
else
 PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
```

### Example of ANSI escape codes

```bash
echo -e "\033[31mThis is red text\033[0m"
```
