# Useful Tmux Cheatsheet
This page documents some useful shortcuts for [Tmux](https://github.com/tmux/tmux/wiki).

The OS is limited to Windows, not Mac.

## Sessions
1. Start a new session
```
$ tmux
$ tmux new
$ tmux new-session
```
2. Start/Kill a named session
```
$ tmux new -s mysession
$ tmux kill-session -t mysession
```
3. Rename session 
`Ctrl` + `b`  `$`

4. Detach from session 
`Ctrl` + `b`  `d`

5. Show all sessions 
```
$ tmux ls
$ tmux list-sessions 
```
`Ctrl` + `b`  `s`

6. Attach to a session 
```
$ tmux a    // attach to last session
$ tmux attach-session    // attach to last session 
$ tmux a -t mysession    // attach to a session named as mysession 
```

## Windows
1. Create window
`Ctrl` + `b`  `c`

2. Close current window
`Ctrl` + `b`  `&`

3. Next window 
`Ctrl` + `b`  `n`

4. Switch/select window by number
`Ctrl` + `b`  `0..9`

## Panes
1. Split pane vertically
`Ctrl` + `b`  `%`

2. Split pane horizontally
`Ctrl` + `b`  `;`

3. Move current pane left
`Ctrl` + `b`  `{`

4. Move current pane right
`Ctrl` + `b`  `}`

