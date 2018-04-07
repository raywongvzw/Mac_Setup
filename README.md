# Mac Setup #

## Brew ##
```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor
brew update
brew analytics off
```

## Brew Cask ##
```bash
brew cask install --appdir="/Applications" \
    alfred \
    android-file-transfer \
    appcleaner \
    bartender \
    Caskroom/versions/java8 \
    citrix-receiver \
    cheatsheet \
    docker \
    firefox \
    flux \
    google-chrome \
    insomniax \
    iterm2 \
    itsycal \
    osxfuse \
    pgadmin4 \
    postman \
    skitch \
    spectacle \
    sts \
    sublime-text \
    the-unarchiver \
    vagrant \
    virtualbox
```

## Brew Install ##
```bash
brew install \
    ag \
    bash \
    chromedriver \
    coreutils \
    curl \
    dos2unix \
    findutils \
    fzf \
    git \
    kubernetes-cli \
    netcat \
    node \
    ntfs-3g \
    openssl \
    python@2 \
    python \
    tree \
    vim \
    wget \
    zsh \
    zsh-completions
```

## Post Brew ##
```bash
sudo dscl . -create /Users/$USER UserShell /usr/local/bin/zsh

git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"

setopt EXTENDED_GLOB
for rcfile in ""${ZDOTDIR:-$HOME}""/.zprezto/runcoms/^README.md(.N); do
  ln -s ""$rcfile"" ""${ZDOTDIR:-$HOME}/.${rcfile:t}""
done

$(brew --prefix)/opt/fzf/install
```

Add into ~/.zpreztorc

```
zstyle ':prezto:load' pmodule \
  'archive' \
  'autosuggestions' \
  'completion' \
  'directory' \
  'docker' \
  'editor' \
  'environment' \
  'git' \
  'history-substring-search' \
  'history' \
  'homebrew' \
  'prompt' \
  'spectrum' \
  'syntax-highlighting' \
  'syntax-highlighting' \
  'terminal' \
  'utility'
zstyle ':prezto:module:prompt' theme 'paradox'
```

Add this into ~/.zshrc
```bash
export HOMEBREW_NO_ANALYTICS=1

# fzf via Homebrew
if [ -e /usr/local/opt/fzf/shell/completion.zsh ]; then
  source /usr/local/opt/fzf/shell/key-bindings.zsh
  source /usr/local/opt/fzf/shell/completion.zsh
fi

# fzf via local installation
if [ -e ~/.fzf ]; then
  _append_to_path ~/.fzf/bin
  source ~/.fzf/shell/key-bindings.zsh
  source ~/.fzf/shell/completion.zsh
fi

# fzf + ag configuration
if _has fzf && _has ag; then
  export FZF_DEFAULT_COMMAND='ag --nocolor -g ""'
  export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
  export FZF_ALT_C_COMMAND="$FZF_DEFAULT_COMMAND"
  export FZF_DEFAULT_OPTS='
  --color fg:242,bg:236,hl:65,fg+:15,bg+:239,hl+:108
  --color info:108,prompt:109,spinner:108,pointer:168,marker:168
  '
fi
```

```bash
defaults write com.apple.dock workspaces-auto-swoosh -bool NO
killall Dock
```
