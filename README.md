# device-setup

homebrew setup:
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' > .zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
brew install neovim ripgrep fzf golang font-ubuntu-{,mono}-nerd-font powerlevel10k kubectl krew podman podman-compose
brew install --cask google-chrome ghostty visual-studio-code podman-desktop
kubectl krew update && kubectl krew install ctx ns
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

# Docs: https://zsh.sourceforge.io/Doc/Release/Options.html
#
# 16.2.1 Changing Directories
setopt AUTO_CD                # Go to folder path without using cd
setopt AUTO_PUSHD             # Push the old directory onto the stack on cd
setopt CD_SILENT              # Never print the working directory after a cd
setopt PUSHD_IGNORE_DUPS      # Do not store duplicates in the stack
setopt PUSHD_SILENT           # Do not print the directory stack after pushd or popd

# 16.2.2 Completion
setopt ALWAYS_TO_END          # If a completion is performed with the cursor within a word move to the end of the word
setopt AUTO_MENU              # Automatically use menu completion after the second consecutive request for completion

# 16.2.3 Expansion and Globbing
setopt EXTENDED_GLOB          # Use extended globbing syntax.

# 16.2.4 History
setopt EXTENDED_HISTORY       # Write the history file in the ':start:elapsed;command' format.
setopt HIST_EXPIRE_DUPS_FIRST # Expire a duplicate event first when trimming history.
setopt HIST_FIND_NO_DUPS      # Do not display a previously found event.
setopt HIST_IGNORE_DUPS       # Do not record an event that was just recorded again.
setopt HIST_IGNORE_ALL_DUPS   # Delete an old recorded event if a new event is a duplicate.
setopt HIST_IGNORE_SPACE      # Do not record an event starting with a space.
setopt HIST_SAVE_NO_DUPS      # Do not write a duplicate event to the history file.
setopt HIST_VERIFY            # Do not execute immediately upon history expansion.
setopt SHARE_HISTORY          # Share history between all sessions.

# Source: https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/completion.zsh
zstyle ':completion:*:*:*:*:*' menu select
zstyle ':completion:*' matcher-list 'm:{[:lower:][:upper:]-_}={[:upper:][:lower:]_-}' 'r:|=*' 'l:|=* r:|=*'
zstyle ':completion:*' special-dirs true
zstyle ':completion:*' list-colors ''
zstyle ':completion:*:*:kill:*:processes' list-colors '=(#b) #([0-9]#) ([0-9a-z-]#)*=01;34=0=01'
zstyle ':completion:*:*:*:*:processes' command "ps -u $USERNAME -o pid,user,comm -w -w"
zstyle ':completion:*:cd:*' tag-order local-directories directory-stack path-directories
zstyle ':completion:*:*:*:users' ignored-patterns \
        adm amanda apache at avahi avahi-autoipd beaglidx bin cacti canna \
        clamav daemon dbus distcache dnsmasq dovecot fax ftp games gdm \
        gkrellmd gopher hacluster haldaemon halt hsqldb ident junkbust kdm \
        ldap lp mail mailman mailnull man messagebus mldonkey mysql nagios \
        named netdump news nfsnobody nobody nscd ntp nut nx obsrun openvpn \
        operator pcap polkitd postfix postgres privoxy pulse pvm quagga radvd \
        rpc rpcuser rpm rtkit scard shutdown squid sshd statd svn sync tftp \
        usbmux uucp vcsa wwwrun xfs '_*'
zstyle '*' single-ignored show

autoload -Uz compinit && compinit
autoload -U +X bashcompinit && bashcompinit

export HISTSIZE=10000
export SAVEHIST=10000
export LANG=en_US.UTF-8
export EDITOR="nvim"
export PATH="$HOME/go/bin:$HOME/.krew/bin:$PATH"

# Source: https://github.com/ohmyzsh/ohmyzsh/blob/master/lib/directories.zsh
alias -g ...="../.."
alias -g ....="../../.."
alias ls="ls -G"
alias la="ls -AG"
alias l="ls -lhG"
alias ll="ls -lhAG"

alias zshrc="nvim ~/.zshrc && source ~/.zshrc"
alias vi="nvim"
alias vim="nvim"
alias g="git"
alias tf="terraform"
alias k="kubectl"
alias kc="kubectl ctx"
alias py="python3"
alias alpine="docker run --rm -it -v ${PWD}:/work alpine"
alias knginx="kubectl run -it --rm nginx --image nginx -- bash"

compdef g=git
compdef k=kubectl

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```

## VSCode

settings.json
```json
{
    "workbench.startupEditor": "none",
    "workbench.activityBar.location": "top",
    "editor.fontSize": 15,
    "files.autoSave": "onWindowChange",
    "editor.fontFamily": "'UbuntuMono Nerd Font', monospace",
    "editor.renderWhitespace": "all",
    "editor.tabSize": 8,
    "vscode_custom_css.imports": ["file:///Users/max/Documents/vscode.css"], 
}
```

/Users/max/Documents/vscode.css
```css
.monaco-editor .suggest-widget .monaco-list .monaco-list-row>.contents>.main>.right>.details-label {
    display: inline !important
}
```

keybindings.json
```json
[
    {
        // vim mode: gt
        "key": "ctrl+tab",
        "command": "workbench.action.showNextWindowTab"
    },
    {
        // vim mode: gT
        "key": "ctrl+shift+tab",
        "command": "workbench.action.showPreviousWindowTab"
    }
]
```

Extension starter pack:
```
- editorconfig
- gitlens
- go
- tailwind
- templ-vscode
- vim
- xml
- yaml
- https://marketplace.visualstudio.com/items?itemName=be5invis.vscode-custom-css
```
