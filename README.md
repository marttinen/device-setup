# device-setup

homebrew setup:
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install --cask google-chrome ghostty visual-studio-code
brew install neovim ripgrep fzf golang font-ubuntu-{,mono}-nerd-font powerlevel10k
```

neovim setup:
```shell
git clone https://github.com/marttinen/kickstart.nvim.git $HOME/.config/nvim
nvim
```

ghostty config:
```
font-family=UbuntuMono Nerd Font
font-size=15
theme=tokyonight
quit-after-last-window-closed=true
```

switch z and y
```shell
hidutil property --set '{"UserKeyMapping": [{"HIDKeyboardModifierMappingSrc":0x70000001D, "HIDKeyboardModifierMappingDst":0x70000001C},{"HIDKeyboardModifierMappingSrc":0x70000001C, "HIDKeyboardModifierMappingDst":0x70000001D}]}'
```

## .zshrc

```shell
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi
source $(brew --prefix)/share/powerlevel10k/powerlevel10k.zsh-theme

export HISTSIZE=10000
export SAVEHIST=10000
export EDITOR="nvim"
export VISUAL="nvim"
export PATH="$HOME/go/bin:$PATH"

alias zshrc="nvim ~/.zshrc && source ~/.zshrc"
alias vi=nvim
alias vim=nvim

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```



