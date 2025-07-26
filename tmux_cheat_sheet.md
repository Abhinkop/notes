# Tmux

## 🏁 Start a New tmux Session
```
tmux new -s demo
```
* new -s demo creates a new session named demo.

## 📐 Split the Window
Inside the tmux session:
### Split Vertically (up/down):
```
Ctrl-b then "
```
### Split Horizontally (left/right):
```
Ctrl-b then %
```

## 🔄 Switch Between Panes
Move between panes using arrow keys:
```
Ctrl-b then → or ← or ↑ or ↓
```

## 📚 Create a New Window
### Create new window (like a new tab):
```
Ctrl-b then c
```
### Switch to next window:
```
Ctrl-b then n
```
### Switch to previous window:
```
Ctrl-b then p
```
### List all windows:
```
Ctrl-b then w
```

## 💾 Detach and Reattach
### Detach from tmux session:
```
Ctrl-b then d
```
### List sessions:
```
tmux ls
```
### Reattach to your session:
```
tmux attach -t demo
```

## 🧹 Kill a Session
### Kill a session from outside:
```
tmux kill-session -t demo
```
### Or from inside:
```
exit
```

## 📝 Optional: Customize tmux (~/.tmux.conf)
### Set prefix to Ctrl-a instead of Ctrl-b
```
unbind C-b
set -g prefix C-a
bind C-a send-prefix
# Enable mouse
set -g mouse on
```
Apply changes:
```
tmux source-file ~/.tmux.conf
```

