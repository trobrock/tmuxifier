# Tmuxifier

Tmuxify your Tmux. Create, edit, mangage and load complex Tmux session, window
and pane configurations with ease.

In short, Tmuxifier allows you to easily create, edit, and load "layout"
files, which are simple shell scripts where you use the `tmux` command and
helper commands provided by tmuxifier to manage Tmux sessions and windows

### Window Layouts

Window layouts create a new Tmux window, optionally setting the window title
and root path where all shells are cd'd to by default. It allows you to easily
split a window into specifically sized panes and more as you wish.

You can load a window layout directly into your current Tmux session, or into
a session layout to have the window created along with the session.

### Session Layouts

Session layouts create a new Tmux session, optionally setting a session title
and root path where all shells in the session are cd'd to by default. Windows
can be added to the session either by loading existing window layouts, or
defined directly within the session layout file.

## Example

Given we have a window layout file called [example.window.sh][] which
looks like:

[example.window.sh]: https://github.com/jimeh/tmuxifier/blob/master/examples/example.window.sh

```bash
window_root "~/Desktop"
new_window "Example Window"
split_v 20
run_cmd "watch -t date"
split_h 60
select_pane 0
```

You can then load that window layout into a new window in the
current tmux session using:

```bash
tmuxifier load-window example
```

Which will yield a Tmux window looking like this:

![example](https://github.com/jimeh/tmuxifier/raw/master/examples/example.window-screenshot.png)

## Installation

```bash
git clone https://github.com/jimeh/tmuxifier.git ~/.tmuxifier
```

And add the following to your `~/.profile`, `~/.bash_profile` or equivalent:

```bash
[[ -s "$HOME/.tmuxifier/init.sh" ]] && source "$HOME/.tmuxifier/init.sh"
```

## Usage

*__Note:__ This section needs expanding upon.*

For a quick reference on available commands and their aliases, please run:

    tmuxifier help

Tmuxifier doesn't come with any layouts, so you'll want to create your own
window and session layout files. New layout files are populated with examples
and comments explaining what things do. Also, having a look at the
[examples][] directory will also give you a good idea.

### Window Layouts

First off you'll want to define a window layout:

    tmuxifier new-window my-awesome-window

This will create a new layout file called `my-awesome-window.window.sh` in
your `$TMUXIFIER_LAYOUT_PATH`, and open it with the editor defined in
`$EDITOR`. Customize it as you wish, and save.

You can now load *my-awesome-window* with the following command:

    tmuxifier load-window my-awesome-window

You should now have a new Tmux window open created from your custom and
awesome window layout.

### Session Layouts

To create your first session layout, run:

    tmuxifier new-session my-awesome-session

Same deal as with creating a new window, except the filename ends with
`.session.sh` instead of `.window.sh`, and the file's pre-populated content
looks different. To have your awesome window loaded, add `load_window
"my-awesome-window"` to the session layout next to existing examples.

To load the session layout simply run:

    tmuxifier load-session my-awesome-session

You'll now have a new Tmux session with your previously defined awesome window
in it.

[examples]: https://github.com/jimeh/tmuxifier/tree/master/examples

## Configure & Customize

### Custom Installaton Path

To install Tmuxifier to a custom path, clone the repository to your desired
path and set `$TMUXIFIER` to that path, additionally loading `init.sh` from
that same path.

```bash
export TMUXIFIER="$HOME/.dotfiles/tmuxifier"
[[ -s "$TMUXIFIER/init.sh" ]] && source "$TMUXIFIER/init.sh"
```

### Custom Layouts Path

You can customize the layouts directory used by Tmuxifier by setting
`$TMUXIFIER_LAYOUT_PATH`.

```bash
export TMUXIFIER_LAYOUT_PATH="$HOME/.tmux-layouts"
```

### Disable Shell-Completion

Tmuxifier comes with shell-completion for bash and zsh. If for any
reason you need to disable it, just set `$TMUXIFIER_NO_COMPLETE`.

```bash
export TMUXIFIER_NO_COMPLETE=1
```

## Inspiration

- Tmuxifier is largely inspired by [Tmuxinator][].
- The shell script structure and shell-completion setup is heavily
  inspired/ripped from the internals of [rbenv][].

## Tmuxifier vs. Tmuxinator

Though Tmuxifier is largely inspired by the excellent [Tmuxinator][] project,
but does set itself apart in a number of ways:

- Uses shell scripts to define Tmux sessions and windows instead of YAML
  files. The benefit is total control over Tmux, but the definition files are
  more complicated to work with.
- Instead of using a "project" concept, Tmuxifier uses a concept of "sessions"
  and "windows" just like Tmux itself. This allows you to load a whole session
  with multiple pre-defined window configurations, or just load a single
  window configuration into your existing session.
- Tmuxifier is a set of shell scripts, meaning it doesn't require Ruby to be
  installed on the machine.

[tmuxinator]: https://github.com/aziz/tmuxinator
[rbenv]: https://github.com/sstephenson/rbenv

## Heed My Warning

Tmuxifier is pretty much an alpha product hacked together over a weekend at
this point. Documentation is sketchy at best, and things might drastically
change and/or break.

But if that doesn't put you off, please enjoy Tmuxifier :)

## Todos

* Improve Readme, specially Usage section.
* Expand `help` command with details for most commands, rather than just the
  essential ones.

## License

(The MIT license)

Copyright (c) 2012 Jim Myhrberg.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
