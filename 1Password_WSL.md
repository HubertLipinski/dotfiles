# 1password setup with WSL

Quick setup for using 1Password SSH keys in WSL via Windows SSH Agent and npiperelay. 

No systemd or PowerShell setup needed.

## Prerequisites
1. WSL2 (Ubuntu or other distro)
2. 1Password Desktop with [SSH Agent enabled](https://developer.1password.com/docs/ssh/get-started/#step-3-turn-on-the-1password-ssh-agent)
3. [npiperelay.exe](https://github.com/jstarks/npiperelay) placed in any of `PATH` folders. Could be `%USERPROFILE% -> C:\Users\username`

## Install `socat` in WSL

```bash 
sudo apt update
sudo apt install -y socat
```


## Configure SSH Agent

Create custom directory for agent.sock
```bash
mkdir -p ~/.1password
```

Edit `~/.bashrc`
```bash
# Path to npiperelay.exe in Windows
export NPIPERELAY_EXE=/mnt/c/Users/<username>/npiperelay/npiperelay.exe

export SSH_AUTH_SOCK=~/.1password/agent.sock
if [ ! -S "$SSH_AUTH_SOCK" ]; then
    socat UNIX-LISTEN:$SSH_AUTH_SOCK,fork EXEC:"$NPIPERELAY_EXE -ei -s //./pipe/openssh-ssh-agent" &
fi
```

Optional alias to restart agent if socket breaks
```bash
alias ssh-agent-restart='pkill socat 2>/dev/null; rm -f $SSH_AUTH_SOCK; source ~/.bashrc'
```

Load changes:
```bash
source ~/.bashrc
```

## Test

```bash
ssh-add -l             # Should list your 1Password SSH keys
ssh -T git@github.com  # Test GitHub connection
```