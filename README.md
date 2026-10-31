# dotfiles

Managed with [chezmoi](https://www.chezmoi.io/). Nothing is tracked yet.

This repo is public, so never commit passwords, tokens, private keys or SSH
config.

## Add files from a laptop

Set up chezmoi and clone this repo to `~/.local/share/chezmoi`:

```sh
brew install chezmoi
chezmoi init cormacrw/dotfiles
```

If this laptop has added files before, get the latest instead:

```sh
chezmoi git pull
```

Add the files you want. Your originals aren't changed:

```sh
chezmoi add ~/.zshrc ~/.gitconfig ~/.config/ghostty/config
```

Check what will be published. The files are named like `dot_zshrc` here:

```sh
chezmoi managed
grep -rnEi 'password|passwd|secret|token|api_?key|sshpass' "$(chezmoi source-path)" --exclude-dir=.git
```

Commit and push:

```sh
chezmoi git add .
chezmoi git -- commit -m "Add dotfiles from <laptop>"
chezmoi git push
```

### Files containing a secret

Add the file as a template, then replace the secret with a variable:

```sh
chezmoi add --template ~/.zshrc
chezmoi edit ~/.zshrc     # replace the secret with {{ .my_password }}
```

Put the real value in `~/.config/chezmoi/chezmoi.toml`, which is never committed,
and do this on every laptop that uses the file:

```toml
[data]
    my_password = "the real value"
```

Run `chezmoi cat ~/.zshrc` to check the file renders correctly.

## Apply to a laptop

`chezmoi apply` overwrites files in `$HOME`, so preview first:

```sh
chezmoi git pull
chezmoi diff
chezmoi apply
```
