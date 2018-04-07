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
    asepsis \
    appcleaner \
    citrix-receiver \
    cheatsheet \
    docker \
    firefox \
    flux \
    google-chrome \
    insomniax \
    iterm2 \
    itsycal \
    java \
    osxfuse \
    pgadmin4 \
    postman \
    skitch \
    spectacle \
    sublime-text \
    the-unarchiver \
    vagrant \
    virtualbox
```

## Brew Install ##
```bash
brew install \
    ack \
    bash
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
Add this your env
```bash
export HOMEBREW_NO_ANALYTICS=1
```

```bash
brew cask alfred link
$(brew --prefix)/opt/fzf/install
```
