# ■■■ Day 20
# Introduction <<<<<
- ctrl + e - scroll window down (window, not cursor)
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
    You can also get to any file in a buffer stack with partical search string
    like this: `:b cat`

    To delete a file from the buffer, type `:bd`, this is a safe command
    it throws an error if you haven't saved the file.
    You should delete the buffer that you are not currently on.
    Otherwise you will exit vim.

    To delete specific buffer in a window type `:bd filename` 
    to get autocompletion, press tab after the space.

2.  Unimpared
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
    within a row/col. (r - rotate)

5. Resizing windows
    ctrl + w + = - make all windows equal
    `:resize +N` - increases the height of the current window by N `rows`
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
        You can also call it just by opening a directory in vim `:e /dir`

        Commands: 
            enter - opens files/dirs
            '-' - go 'up' directory
            D - delete f/d
            R - rename f/d

            :Vex - open in vertical split
            :Sex - open in horisontal split
            :Lex - left full-height vertical split
    2. :e with wildMenu
        Another way to browse files is with `set wildmenu` in vimrc
        It shows you possible files in your directory on tab in `:e ` command
        
        When you focus on directory you can press down arrow to go down 
        the directory or you can always press up arrow to go back.

        `set wildmode=list:longest,full` - allows you to autocomplete to the 
            longest match possible on a first tab, and traverse files on a second tab.

    3. NERDTree
       Call - `:NERDTree'
       Move - h/j/k/l
       Open - enter/o, it opens files in the last created window
       
       Adding bookmark - place a cursor on a dir, execute `:Bookmark`
       To toggle bookmarks - press B.
       
       To enable nerdtree on startup add to your vimrc `autocmd VimEnter * NERDTree`

    4. Vinegar
       It allows you to open file tree in the windows you are currently in.
       To open Ftree, press `-`
       Before opening Ftree don't forget to save changes to file.

    5. CtrlP
       Is a fuzzy completion plugin that allows you to open files quickly in case you know 
       what you are looking for. 

       To find files, hit Ctrl+p.
       To navigate files, press Ctrl + j/k
       To open file, press enter
       To close CtrlP window, press esc

       This plugin also allows you to navigate through buffers - ctrl + b
       and most recently used files - ctrl + f (first you have to open ctrlP window)

9. Navigating text
    F/f - search character in line back/forward
    T/t - search until character
    (_/^)/$ - go to start/end of the non-blank line
    0 - (zero) - go to the very beginning of the line
    ge - go to the end of the previous word
    
    ctrl + b/f - scroll one page back/forward
   
    (/) - go back/forward one sentence.
    
    To see line numbers add `set numbers` to vimrc
    To jump to specific line:
        vim filename +N (plus is mandatory)
        ex:N
    To jump relatively:
        ex:+/-N
    To display numbers relatively - `set relativenumber`

10. Jumping into insert mode
    a/A/i/I/o/O - after/EOL/before/BOL/below/above
    gi - insert where you last edited
    C - detele to the right of the cursor
    cc/S - same, delete the whole line
    s - delete a single character

11. Searching with / and ?
    `set hlsearch` - highlights every match.
    `set incsearch` - move to the next match as soon as you type
    
12. Searching across files
    There are two commands for this - `:grep` and `:vimgrep`
    vimgrep - for novices, grep for pros

    How to use vimgrep: 
        syntax: `:vimgrep <pattern><path>
        pattern - can be a string or regex
        path - normally it's `**`, but to restrict filetype - `**/*.js`
        
        to navigate between matches execute - `:copen`, navigate by j/k and 
        press enter to go to match.

13. ack
    you can use this `linux` package to search through code bases.
    this package is a spiritual successor of grep
    install - `sudo apt-get install ack-grep`
    to integrate with vim quickfix window, install plugin `ack.vim`
        execute - `:Ack --python Animal`, which will run ack and populate qf window
        which you will be able to see with `:copen`
        
14. Using text objects
    Text objects are only available in combination with other commands.
    e.g:
        d2aw, ci)
        
    Characters commonly used in pairs in programming are all supported.
    e.g: ', ", {, [

15. Easymotion (plugin)
    To invoke the plugin heat the leader key twice ('\')
    After that press w.

    This plugin supports the following commands:
        f, t, w, b, k, j, n (based on the last search)

    You can customize other keys, to search for what you want.

16. Copying and pasting with registers
    y - copy character/selection
    yy - copy line
    y<text obj(w,e,{)> - copy text obj

    p - paste what you have copied

    when you copy text it's saved in `registers`
    You can access registers by hitting(not exec) "<reg_name>
    then you can copy or paste to registers, by putting 
    p or c after `"<reg_name>`.

    "Unnamed" register can be accessed with double empty quotes.

    E.g to paste from a register you can press `"ap`
    to paste from unnamed register - `""p` which is the same as just p.
    
    Numbered registers - history of your delete operations.

    To see register content - execute `:reg`
    to see specific register content - `:reg a`

17. Copying from outside
    register called `*` is system clipboard (not in linux)
    in linux - `+`
    
# Plugin management
## Managing plugins
The newest plugin manager is vim-plug
To add plugin, paste path from github domain to the plugin
between calls to plugin manager.

To install listed plugins execute `:PlugInstall`
To update plugins - `:PlugUpdate`
To delete plugins that are no longer in your vimrc - `:PlugDelete`

1. Lazy loading
    To load on execution command - `Plug 'pathtoplugin', {'on': 'NERDTreeToggle'}
    To load for special filetype - xxxxxxxxxxxxxxxxxx, {'for': 'markdown'}
    
## Plugin performance profiling
You can use --startuptime flat when calling vim.

## Deep dive into modes
1. Command line mode
    How to enter? - hit colon `:`, or `/` or `?`
    Traverse history - arrows OR ctrl+p/ctrl+n

    Open history buffer - ctrl+f
    Closer history buffer - ctrl+c
    Execute command from history buffer - enter
    
2. Insert mode
3. Visual mode
    v, V, ctrl+v - char, line, block visual modes
    Expand selection from another side - o
    Discard selection - esc

4. Replace mode
    R - enter replace mode
    This mode replaces each character.

5. Terminal mode
    `:term` - enter terminal mode
    This mode allows you to execute terminal commands in a separate window
    
    To move from the termila window you can still use ctrl+w+h command.

    You can get output form terminal command in a buffer like this:
        `:term python3 app.py input1 input2`
        result will be awailable to you in a horizontal split

    To exit terminal mode, press `ctrl + w` then type `:q!`

## Remapping commands
For remapping, vim provides two commands
    :map - recursive mapping
    :noremap - non recursive mapping

map commands are aware of other custom mappings, while noremap 
works with system defaults

To see other mapped commands defined by you or your plugins,
use `:map` command like this - `:map g` - shows every mapping starting with g.

When we remap ; to : we should use `noremap` because we still want to
enter command mode on ; even after colon gets remapped to something else

To map save to ctrl + s - `noremap <c-s> :w<cr>` Note, <cr> is carriage return.
To map alt - `<a-x>`
To map shift - `<s-x>`

You can map to almost all different keys

## The leader key
Leader key is a namespace for user/plugin defined shortcuts.
Default is backslash, but it's common in community to remap it to comma.

