# dotfiles

Managed with [chezmoi](https://www.chezmoi.io/).

## Setup on a new machine

```bash
sh -c "$(curl -fsLS get.chezmoi.io)" && ~/bin/chezmoi init HubertLipinski --apply
```

During setup you will be prompted for:

| Option | Description |
|--------|-------------|
| **Host type** | `linux`, `wsl`, `mac`, or `windows` |
| **npiperelay path** | Path to `npiperelay.exe` (WSL only, see [1Password WSL setup](1Password_WSL.md)) |
| **GitHub name** | Git commit author name |
| **GitHub email** | Git commit author email |
