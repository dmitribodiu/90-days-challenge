# ■■■ Day 20
# Introduction <<<<<
- ctrl + e - scroll window down
- ctrl + y - scroll window up

## Plugins
- nerdTree - see files
- ctrlp - file finder (as ctrl+p in vsc)
- fugitive - git tool
- youcompleteme vs coc

## Tmux
Tmux is a terminal multiplexer. It allows you to view and controle
multiple consoles.

# Mastering Vim <<<<<
# Getting Started
To see where vim reading .vimrc from, type `:echo $MYVIMRC`.

To try a command before adding it to your vimrc, type `:set autoindent`
`:set autoindent?` - with question mark will tell you the current value
`:colorscheme + tab` - see different schemes

## Opening file
`vim filepath` - to open a file
`:e filepath` - open file from vim

## Saving and closing files
`:w` - save
`:w filename` - save as
`:q` - quit
`:q!` - discard changes

## Swap files
Vim creates swap files when you edit. If you don't exit
vim cleanly, you may see a message, telling you about 
swap files. If you don't like vim littering your directories
with swap files you can tell it to store all swap files in 
a single directory, add to vimrc - `set directory=$HOME/.vim/swap/`

## Moving around
- { / }  - move by a paragraph (paragraph is text separated by 
    two new lines)

## Commands combinations
All commands follow this `pattern`: command:number:movement/text_obj

## Persistent undo and repeat
- u - undo
- ctrl + r - redo

## Read manual
You can read manual for a command with `:help command` in vim.
You can also use a shortcut - `:h`

# Advanced editing and navigation
## Installing plugins
1. Creating a directory to store plugins: mkdir -p ~/.vim/pack/plugins/start
2. Tell vim to load docs for your plugins, add to your vimrc
    packloadall
    silent! helptags ALL
3. Now every time to add a plugin, you will need to run the following command
    git clone https://github.com/username/packagename 
        ~/.vim/pack/plugins/start/packagename

## Organizing workspace
`Buffers` are the way Vim internally represents files, they allow you to
switch quickly between files.

1. Buffers
    To see list of existing buffers type `:ls`
        1 - buffer number
        % - buffer is in the current window
        a - buffer is active
    When you open another file with `:e` it will be loaded on the buffer stack
    You will be able to access previous buffer with `:b1` command
    You can also got to any file in a buffer stack with partical search string
    like this: `:b cat`

    To delete a file from the buffer, type `:bd`, this is a safe command
    it throws an error if you haven't saved the file.

2. Plugin spotlight - unimpared
    Allows you to quickly swap buffers with the following shortcuts
        1. ]b [b - cycle throught buffers
        2. [f ]f - cycle throught files in the same directory as the current buffer
        3. ]/]l - cycle through location list
        4. [/]t - cycle through tags
    To see the whole list, type :h unimpaired

3. Managing Windows
    One way to split windows is with the following command `:split filepath`
    Shortcut for split is :sp
    Splitting vertically can be attained with `:vs filepath`

    To `move` between windows use `ctrl + w + direction_withHJKL`, e.g : `ctrl + w + h`
        You can map it to `ctrl + h` in your vimrc
    To `close` the window you can use `ctrl + w + q` 
        `:bd` will close the current window and delete the current buffer
        `ctrl + w + o` will close all other windows except the current one
            but only if other windows don't contain changes

4. Moving windows
    To get help, type `:h window-resize`
    As other window shortcuts these a prefixed with ctrl + w:
        - H - move to the left of the screen
        - J
        - K
        - L
    You can also choose a content for each window with the buffer command `:b`
    e.g `:b cat`

    You can move windows with `ctrl + w + r` command which shifts every window
    within a row/col.

5. Resizing windows
    ctrl + w + = - make all windows equal
    `:resize +N` - increases the height of the current window by N `columns`
    `:vertical resize` - increases or decreases the width
    shortcuts - res and vert res
    
6. Tabs
    Vim uses tabs to switch between `collections of windows` as opposed to
    switching between files. This allows you to have multiple `workspaces`

    Tabs are useful when you need to change context of your work. (e.g back to front end)

    To open a new tab, execute the following `:tabnew`
    To open a new tab with a file - `:tabnew filename`

    Navigation between files is a piece of cake - `gt and gT` for 
        next/prev tab. 
    Tabs can be closed with `:tabclose` or by closign all the windows it contains.

    To `move` a tab use `:tabmove N` to place the current tab after the Nth tab.

7. Folds
    Folds is a powerful feature for navigating large files.
    
    `Tip` to update your vimrc, execute `:source $MYVIMRC`

    To set fold method in your vimrc add `set foldmethod=indent`
    Supported fold methods, there are multiple of them, but you only need indent.
    To unfold, type `zo` - this will open the current fold
    To fold, type `zc` - this will close the fold (z looks like folded line)
    To toggle fold, type `za`

    To open/close all fols in the file you can use `zR and zM` respectively

8. Navigating file trees
    1. Netrw
        Is a built-in file manager in Vim

        Use `:Ex[plore]` to open file navigation window.

