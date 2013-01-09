## Installation
`tmux` is available through most package managers.

```bash
brew install tmux
port install tmux
sudo apt-get install tmux
```

## Basics
Start an unnamed `tmux` session and connect to it:
```bash
tmux
```
Start a named session:
```bash
tmux start-session -s <name>
```
List tmux sessions:
```bash
tmux list-sessions
```
Re-attach to a session (sessionname optional, connects to last if omitted):
```bash
tmux attach [-s sessionname]
```
Destroy a session:
```bash
tmux destroy [-s sessionname]
```

All available commands are detailed in `man tmux`.

## Colors
I prefer the solarized color scheme (in `tmux` and just about everything else!)

```bash
mkdir ~/.tmux
git clone https://github.com/seebi/tmux-colors-solarized.git ~/.tmux/tmux-colors-solarized
echo "source ~/.tmux/tmux-colors-solarized/tmuxcolors-256.conf" >> ~/.tmux.conf
```

## Powerline
A recent version of bash is required (>=3.2). Mountain lion may or may not come with it, I recommended that you `brew install bash` and follow the `caveats` section to install the latest version.

Also be sure to install a patched font. A bunch of patched fonts exist at https://github.com/Lokaltog/vim-powerline/wiki/Patched-fonts, but you can always follow the instructions at https://github.com/Lokaltog/vim-powerline/tree/develop/fontpatcher to patch another one you like.
```bash
git clone https://github.com/erikw/tmux-powerline.git ~/.tmux/tmux-powerline
```

Add the following to your ~/.tmux.conf file: (subject to change, check the tmux-powerline README.md)
```conf
set-option -g status on
set-option -g status-interval 2
set-option -g status-utf8 on
set-option -g status-justify "centre"
set-option -g status-left-length 60
set-option -g status-right-length 90
set-option -g status-left "#(~/.tmux/tmux-powerline/powerline.sh left)"
set-option -g status-right "#(~/.tmux/tmux-powerline/powerline.sh right)"
```

If characters still seem a bit off, make sure your terminal is configured for utf8 characters. Add the following to your bash config (~/.bashrc or similar):
```bashrc
export LC_ALL="en_US.UTF-8"
export LANG="en_US"
```

Remember that powerline segments are just bash scripts, and if you are experienced in bash scripting you can create your own segments!

## Enable Mouse
Make sure that you have xterm mouse reporting enabled (under Terminal in iTerm2 settings), then add the following to your ~/.tmux.conf file:
```conf
set-option -g mouse-select-pane on
set-option -g mouse-select-window on
set-window-option -g mode-mouse on
```

## Change <Prefix>
You can change prefix to be whatever you wish, for example Ctrl+A to match `screen`:
```conf
unbind C-b
set -g prefix C-a
```

## Maximize Pane (HACK!)
This is a clever hack to be able to maxmize a pane:
```conf
unbind +
bind + new-window -d -n tmux-zoom \; swap-pane -s tmux-zoom.0 \; select-window -t tmux-zoom
unbind -
bind - last-window \; swap-pane -s tmux-zoom.0 \; kill-window -t tmux-zoom
```

This lets you `<prefix> +` to maximize your current pane (it moves the pane to a new window and renames it). Use `<prefix> -` to minimize it again.

One thing to keep in mind when using this hack is that it relies on the last pane as a bookmark, so when you go to minimize it again it will replace your last used pane with it, destroying that pane. Be a little careful!  You can also only maximize one pane at a time this way.

## tmuxinator
Installation:
```bash
[sudo] gem install tmuxinator
```
(The sudo is only required if your rubygems require you to run as root. I blame Apple.)

Create a new project:
```bash
tmuxinator open <project-name>
```

Start a project:
```bash
tmuxinator start <project-name>
```

Personally, I have a 'pardot' project that I have aliased to `pardot`, and has proven to be very handy.

## Other
My settings: https://github.com/anthonymarion/dotfiles/blob/master/.tmux.conf
These settings also have a block dedicated to being able to switch panes using <Opt-#> without the prefix.

If you need access to the osx clipboard, then follow the instructions at https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard

If you want to see what keybindings exist, you can `<prefix> ?` to see a list (`q` to exit the list).
