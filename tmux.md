# tmux

> tmux is an open-source **terminal multiplexer** for Unix-like operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window. It is useful for **running more than one command-line program** at the same time. It can also be used to **detach processes from their controlling terminals**, allowing remote sessions to remain active without being visible.

from Wikipedia

## Ref

[tmux shortcuts & cheatsheet (github.com)](https://gist.github.com/MohamedAlaa/2961058)

[Basic tmux Tutorial - Windows, Panes, and Sessions over SSH - YouTube](https://www.youtube.com/watch?v=BHhA_ZKjyxo)

[Basic tmux Tutorial, Part 2 -- Shared Sessions - YouTube](https://www.youtube.com/watch?v=norO25P7xHg)

## Basic usage

`tmux`: Enter tmux.

Prefix `<ctrl> + b`.

### Create

`tmux new-session -s session_name`: Create a new session and enter it (`new-session` can 
also be `new`).

`tmux new-window -n window_name`: Create a new window in the last session you were in
without entering it.

### List

`tmux ls`: List sessions (`ls` can also be `list-session` or `list-sessions`).

`tmux list-window`: List windows of the last session you were in (`list-window` can also
be `list-windows`).

### Enter

`tmux a -t session_name`: **A**ttach **t**o an existing session. `attach` can be `at` or
`attach`. Leaving `-t` will send you back to the last window you were at.

### Kill

`tmux kill-session`: Kill the last sessions you were in.

`tmux kill-session -a`: Kill all the sessions except the last session you were in.

`tmux kill-window`: Kill the last window you were in of the last session you were in.

`tmux kill-window -a`: Kill all the windows except the last window you were in of the last
session you were in.

### Other stuff

`tmux split-window`: Split window into two panes, `-v` for vertical split, `-h` for
horizontal split.


## Running command inside tmux sessions

Type `:` and you can run tmux command inside tmux sessions. For example, by default,
directly running `tmux` (or `tmux new`, etc.) inside a tmux session creates nested 
sessions and is disabled by default, but if you type `:new`, a new session will be 
created as if you do this outside of tmux sessions.

## Shortcuts on session level

`s`: List sessions. This leads you to a srollable and selectable list you can choose from.
When you are in this list, use `<Esc>` or `q` to exit, use `j` or `k` to move up or down, 
use `h` and `l` to attach or expand while moving up or down (equivalent to direction
keys).

`d`: Detach from tmux server.

## Shortcuts on window level

`c`: Create a new window.

`,`: Rename window. The default name is `bash` or `zsh`, etc.

`p` and `n` for switching to **p**revious and **n**ext window respectively.

`&`: Kill window (confirm needed).

`w`: List windows.

`f`: Find window.

`0~9`: Switch to corresponding window.

## Shortcuts on pane level

`%`: Create new pane by vertical split.

`"`: Create new pane by horizontal split.

`q`: Display pane indices. Type corresponding number while the indices are displayed to
switch to that pane (only work for single digit index).  

`o`: Switch to the next pane.

`{`: Move current pane left.

`}`: Move current pane right.

`z`: Zoom in/out.

`<space>`: Toggle between different default layouts.

`!`: Break current pane into a new window.

`x`: Kill current pane.
