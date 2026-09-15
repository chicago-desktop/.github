# Chicago

**A desktop that runs entirely inside a terminal — over plain SSH, no
browser, no X server, no VM — in the look of the mid-nineties desktops.**

The window frames, the Start menu, the taskbar with a tray and a clock,
desktop icons, dialogs and menus are drawn with real pixels (the Kitty graphics protocol or
Sixel), so WezTerm, kitty, foot, mlterm or any Sixel-capable terminal show the
chrome pixel for pixel. Everything inside a window stays text cells, so bash,
htop or vim run inside a "window" unchanged. A terminal without graphics gets
a pure text-cell theme of the same desktop.

It is built on [Wippy](https://github.com/wippyai/runtime), a Lua runtime
with a process manager and an actor model: every window, widget, tray
service and dialog is its own process with its own memory and mailbox, the
compositor is one more process that owns the screen, and a crashing window
is a dead process the desktop notices — not a crashed desktop.

## Running it

The pixel chrome needs the runtime's `gfx` module, which is not in a Wippy
release yet; until it is, the shell runs on a build of the
[runtime fork](https://github.com/chicago-desktop/runtime) (`wippy-projects`
branch), and the fork's [releases](https://github.com/chicago-desktop/runtime/releases)
are built by CI. The mechanics (`tui-desktop`) run on a released runtime.

The icon set is being replaced with original pixel art drawn for the project;
until then the packages ship an interim set. The code is MIT.

## Writing a window

A window is a Lua table with `init`, `view` and `update`; `view` returns a
tree of components (menu, list, table, tree, editor, tabs, buttons, gauges)
and the shell draws it in cells or pixels. A registry entry puts it in the
Start menu; a widget is the same shape with an `interval` and no input. The
SDK guide lives in the [shell](https://github.com/chicago-desktop/shell)
repository, and [module-template](https://github.com/chicago-desktop/module-template)
is the starting point for a module of your own: use the template, `make init`,
write your window, `make test`, `make publish`.
