# Mac Setup #

## XCode CLI ##
```bash
xcode-select --install
```

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
    cyberduck \
    docker \
    firefox \
    flux \
    gitkraken \
    google-chrome \
    insomniax \
    iterm2 \
    itsycal \
    osxfuse \
    pgadmin4 \
    postman \
    pycharm-ce \
    skitch \
    spectacle \
    sts \
    sublime-text \
    the-unarchiver \
    vagrant \
    virtualbox \
    visual-studio-code
```

## Brew Install ##
```bash
brew install \
    ag \
    bash \
    chromedriver \
    cmake \
    coreutils \
    curl \
    dos2unix \
    findutils \
    fzf \
    git \
    kubernetes-cli \
    maven \
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
# setup brew zsh as default shell
sudo dscl . -create /Users/$USER UserShell /usr/local/bin/zsh

# zsh manager
git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"

# zsh dot files
setopt EXTENDED_GLOB
for rcfile in ""${ZDOTDIR:-$HOME}""/.zprezto/runcoms/^README.md(.N); do
  ln -s ""$rcfile"" ""${ZDOTDIR:-$HOME}/.${rcfile:t}""
done

# fzf
$(brew --prefix)/opt/fzf/install

# global git ignore
git config --global core.excludesfile ~/.gitignore_global

# vim plug
npm i npm
curl -fLo curl ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# vim dirs
mkdir -p ~/.vim/swaps
mkdir -p ~/.vim/backups
mkdir -p ~/.vim/undo
mkdir -p ~/.vim/plugged

# pip
mkdir -p ~/.pip

pip3 install \
ansible \
autopep8 \
aws \
boto \
boto3 \
bpython \
bs4 \
cfn_flip \
cfn-lint \
lxml \
pipenv \
pylint \
requests

# you complete me vim plugin
cd ~/.vim/plugged	
git clone https://github.com/Valloric/YouCompleteMe.git
git submodule update --init --recursive
./install.py --js-completer --java-completer

# show all files in finder
defaults write com.apple.finder AppleShowAllFiles TRUE;killall Finder
```

Add into ~/.zpreztorc

```
# zsh modules
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
  
# zsh theme
zstyle ':prezto:module:prompt' theme 'paradox'
```

Add this into ~/.zshrc
```bash
# brew
export HOMEBREW_NO_ANALYTICS=1
# Only need for coreutils. Everything else is in /usr/local/bin
# But use gfind, ggrep, etc. Starts with a 'g'
BREW_PATHS=$(brew --prefix coreutils)/libexec/gnubin
PATH=$BREW_PATHS:/usr/local/bin:/usr/local/sbin:$PATH
PATH=/Applications/Postgres.app/Contents/Versions/9.4/bin:$PATH
export PATH

# Java and Maven
M2_HOME=/usr/local/Cellar/maven/3.5.4/libexec
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home
PATH=$JAVA_HOME/bin:M2_HOME/bin:$PATH
export PATH

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

# aliases
alias prune='docker system prune -af'
alias bu='brew update; brew upgrade; brew cleanup; brew doctor'
alias pip2upgrade="pip2 freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs pip2 install -U"
alias pip3upgrade="pip3 freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs pip3 install -U"
```

Add into .vimrc

```bash
call plug#begin('~/.vim/plugged')
Plug 'Valloric/YouCompleteMe'
Plug 'vim-scripts/indentpython.vim'
Plug 'scrooloose/syntastic'
Plug 'nvie/vim-flake8'
call plug#end()

" Make Vim more useful
set nocompatible
" Use the OS clipboard by default (on versions compiled with `+clipboard`)
set clipboard=unnamed
" Enhance command-line completion
set wildmenu
" Allow cursor keys in insert mode
set esckeys
" Allow backspace in insert mode
set backspace=indent,eol,start
" Optimize for fast terminal connections
set ttyfast
" Add the g flag to search/replace by default
set gdefault
" Use UTF-8 without BOM
set encoding=utf-8 nobomb
" Change mapleader
let mapleader=","
" Don’t add empty newlines at the end of files
set binary
set noeol
" Centralize backups, swapfiles and undo history
set backupdir=~/.vim/backups
set directory=~/.vim/swaps
if exists("&undodir")
	set undodir=~/.vim/undo
endif

" Don’t create backups when editing files in certain directories
set backupskip=/tmp/*,/private/tmp/*

" Respect modeline in files
set modeline
set modelines=4
" Enable per-directory .vimrc files and disable unsafe commands in them
set exrc
set secure
" Enable line numbers
 set number
" Enable syntax highlighting
syntax on
" Highlight current line
 set cursorline
" Show “invisible” characters
" set lcs=tab:▸\ ,trail:·,eol:¬,nbsp:_
" set list
" Highlight searches
set hlsearch
" Ignore case of searches
set ignorecase
" Highlight dynamically as pattern is typed
set incsearch
" Always show status line
set laststatus=2
" Enable mouse in all modes
"set mouse=a
" Disable error bells
" set noerrorbells
" Don’t reset cursor to start of line when moving around.
set nostartofline
" Show the cursor position
 set ruler
" Don’t show the intro message when starting Vim
set shortmess=atI
" Show the current mode
set showmode
" Show the filename in the window titlebar
set title
" Show the (partial) command as it’s being typed
set showcmd
" Use relative line numbers
" if exists("&relativenumber")
"	set relativenumber
"	au BufReadPost * set relativenumber
"endif
" Start scrolling three lines before the horizontal window border
set scrolloff=3

" pep8
set textwidth=79  " lines longer than 79 columns will be broken
set shiftwidth=4  " operation >> indents 4 columns; << unindents 4 columns
set tabstop=4     " a hard TAB displays as 4 columns
set expandtab     " insert spaces when hitting TABs
set softtabstop=4 " insert/delete 4 spaces when hitting a TAB/BACKSPACE
set shiftround    " round indent to multiple of 'shiftwidth'
set autoindent    " align the new line indent with the previous line

" Strip trailing whitespace (,ss)
function! StripWhitespace()
	let save_cursor = getpos(".")
	let old_query = getreg('/')
	:%s/\s\+$//e
	call setpos('.', save_cursor)
	call setreg('/', old_query)
endfunction
noremap <leader>ss :call StripWhitespace()<CR>
" Save a file as root (,W)
noremap <leader>W :w !sudo tee % > /dev/null<CR>

" Automatic commands
if has("autocmd")
	" Enable file type detection
	filetype on
	" Treat .json files as .js
	autocmd BufNewFile,BufRead *.json setfiletype json syntax=javascript
	" Treat .md files as Markdown
	autocmd BufNewFile,BufRead *.md setlocal filetype=markdown
    " Enable all Python syntax highlighting features
    autocmd BufRead,BufNewFile *.py let python_highlight_all=1
endif

" YouCompleteMe
let g:ycm_python_binary_path = '/usr/local/bin/python3'
let g:ycm_autoclose_preview_window_after_completion=1
map <leader>g  :YcmCompleter GoToDefinitionElseDeclaration<CR>
```

Reload .vimrc and inside vim, :PlugInstall to install plugins, :PlugUpdate to update plugins.

```bash
defaults write com.apple.dock workspaces-auto-swoosh -bool NO
killall Dock
```

Prevent Wifi from dropping upon lock
```bash
cd /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources && \
sudo ./airport $(ifconfig | grep -B 6 'status: active' | head -n 1 | cut -d : -f 1) prefs DisconnectOnLogout=NO
```

Add into ~/.pip/pip.conf
```bash
[list]
format=columns
```

Add into ~/.gitignore_global
```bash
#######################################
# Operating Systems                   #
#######################################

# OS X
.DS_Store
._*
.Spotlight-V100
.Trashes

# KDE directory preferences
.directory

# Windows
Desktop.ini
Thumbs.db
ehthumbs.db
$RECYCLE.BIN/
Icon?


#######################################
# Temporary                           #
#######################################

# Files
*.tmp
*.log

# Directories
log/
logs/
tmp/
cache/


#######################################
# Archives                            #
#######################################

*.7z
*.bz2
*.bzip
*.deb
*.dmg
*.egg
*.gem
*.gz
*.iso
*.lzma
*.rar
*.rpm
*.tar
*.xpi
*.xz
*.zip


#######################################
# IDE and Editors                     #
#######################################

# Eclipse
*.pydevproject
.project
.metadata
bin
bin/**
*~.nib
local.properties
.classpath
.settings/
.loadpath
.externalToolBuilders/
*.launch
.cproject
.buildpath

# Netbeans
nbproject/private/
build/
nbbuild/
dist/
nbdist/
nbactions.xml
nb-configuration.xml

# IntelliJ IDEA
*.iml
*.ipr
*.iws
.idea/

# XCode
xcuserdata
project.xcworkspace

# Notepad++
nppBackup

# SublimeText project files
*.sublime-workspace

# Textmate
*.tmproj
*.tmproject
tmtags

# vim
.*.sw[a-z]
*.un~
Session.vim
.netrwhist

# vscode
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json

# Editors with AutoBackup
*~


#######################################
# Build Systems                       #
#######################################

# Autotools
Makefile.in
autom4te.cache
aclocal.m4
compile
configure
depcomp
install-sh
missing

# waf
.waf-*
.lock-*

# Maven
target
pom.xml.tag
pom.xml.releaseBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml


#######################################
# Version Control Systems             #
#######################################

# Mercurial
/.hg/*
*/.hg/*
.hgignore

# SVN
.svn/


#######################################
# Languages                           #
#######################################

# Java
*.class
*.ear
*.jar
*.war

# C
*.o
*.lib
*.a
*.dll
*.ko
*.so
*.so.*
*.dylib
*.exe
*.out
*.app

# C++
*.slo
*.lo
*.o
*.so
*.dylib
*.lai
*.la
*.a

# Latex
*.acn
*.acr
*.alg
*.aux
*.bcf
*.bbl
*.blg
*.brf
*.dvi
*.fdb_latexmk
*.fls
*.glg
*.glo
*.gls
*.idx
*.ilg
*.ind
*.ist
*.lof
*.log
*.lol
*.lot
*.maf
*.mtc
*.mtc0
*.nav
*.nlo
*.out
*.pdfsync
*.pdftex
*.ps
*.pyg
*.snm
*.synctex.gz
*.thm
*.toc
*.vrb
*.xdy
*.tdo

# Python
*.pyc

# Scala - sbt specific
.cache
.history
lib_managed/
src_managed/
project/boot/
project/plugins/project/
project/target

# Scala - IDE specific
.scala_dependencies
.worksheet


#######################################
# Server                              #
#######################################

cgi-bin/
error_log
.bash_logout
.bash_profile
.bashrc
.cgi-bin/
.contactemail
.cpanel/
.cpanel-logs
.ftpquota
.htpasswd
.htpasswds
.HttpRequest/
.lastlogin
.rnd
.trash/


#######################################
# Caches                              #
#######################################

# SASS
.sass-cache


#######################################
# File backups                        #
#######################################

*.bak
*.backup

#######################################
# Ansible                             #
#######################################

*.retry
```

## vscode ##
Preferences
```
CMD + ,
"telemetry.enableTelemetry": false,
"telemetry.enableCrashReporter": false
```

Extensions
```
Ansible
autoDocString
Bracket Pair Colorizer
Code Spell Checker
Docker
Log File Highlighter
Markdown All in One
Markdown Preview Enhanced
markdownlint
Path Intellisense
Python
Python-autopep8
Trailing Spaces
vscode-ansible-linter
vscode-cfn-lint
YAML Support by Red Hat
```

Settings
```
CMD + Shift + P
Python: Select Interpreter

```
