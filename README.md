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

curl -fLo curl ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
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

```bash
defaults write com.apple.dock workspaces-auto-swoosh -bool NO
killall Dock
```
