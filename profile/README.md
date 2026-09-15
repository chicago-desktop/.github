# Wippy Windows

**A Windows 95 desktop that runs entirely inside a terminal — over plain SSH,
no browser, no X server, no VM.**

Windows, the Start menu, the taskbar with a tray and a clock, desktop icons,
dialogs and menus are drawn with real pixels (the Kitty graphics protocol or
Sixel), so Windows Terminal, WezTerm, kitty, foot or mlterm show the 1995
chrome as it was. Everything inside a window stays text cells, so bash, htop
or vim run inside a "window" unchanged. A terminal without graphics gets a
pure text-cell theme of the same desktop.

It is built on [Wippy](https://github.com/wippyai/runtime), a Lua runtime
with a process manager and an actor model: every window, widget, tray
service and dialog is its own process with its own memory and mailbox, the
compositor is one more process that owns the screen, and a crashing window
is a dead process the desktop notices — not a crashed desktop.

## Repositories

| Repository | What it is |
|---|---|
| [tui-desktop](https://github.com/wippy-windows/tui-desktop) | The mechanics: a compositor of overlapping windows for the terminal, real programs under a PTY, one desktop per SSH connection, a command channel an agent can drive. No look of its own — a theme brings it. |
| [windows](https://github.com/wippy-windows/windows) | The Windows 95 shell on top: the pixel chrome, the Start menu, the logon screen, a declarative window SDK, Notepad, My Computer with one window per folder, Display Properties, Task Manager, desktop widgets, aICQ (an ICQ-style list of people and AI agents) and AntiBug (a 1995-antivirus-style test scanner). |
| [runtime](https://github.com/wippy-windows/runtime) | A fork of [wippyai/runtime](https://github.com/wippyai/runtime) carrying what the desktop needs before it lands upstream: the `gfx` module (pixels over Kitty/Sixel), the `terminal.ssh` host (one desktop per SSH connection) and terminal fixes. Work lives on the `wippy-projects` branch. |
| minesweeper | Minesweeper as a separate module on the window SDK *(being extracted)*. |
| weather | A weather window, a tray temperature and a desktop widget as a separate module *(being extracted)*. |

## Running it

The pixel chrome needs the runtime's `gfx` module, which is not in a Wippy
release yet; until it is, the shell runs on a build of the
[runtime fork](https://github.com/wippy-windows/runtime) (`wippy-projects`
branch).
The mechanics (`tui-desktop`) run on a released runtime.

The shell repository is private for now: it ships the original Windows 95
icon set, which is Microsoft's artwork and not ours to redistribute. A
release will carry its own icons.

## Writing a window

A window is a Lua table with `init`, `view` and `update`; `view` returns a
tree of components (menu, list, table, tree, editor, tabs, buttons, gauges)
and the shell draws it in cells or pixels. A registry entry puts it in the
Start menu; a widget is the same shape with an `interval` and no input.
See the SDK guide in the shell repository.
