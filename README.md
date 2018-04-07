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
    ack \
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
```

```bash
defaults write com.apple.dock workspaces-auto-swoosh -bool NO
killall Dock
```
