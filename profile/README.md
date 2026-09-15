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

| Repository | Hub module | What it is |
|---|---|---|
| [tui-desktop](https://github.com/wippy-windows/tui-desktop) | `windows/tui-desktop` | The mechanics: a compositor of overlapping windows for the terminal, real programs under a PTY, one desktop per SSH connection, a command channel an agent can drive. No look of its own — a theme brings it. |
| [windows](https://github.com/wippy-windows/windows) | `windows/shell` | The Windows 95 shell on top: the pixel chrome, the Start menu, the logon screen, a declarative window SDK, Notepad, My Computer with one window per folder, Display Properties and desktop widgets. Everything else is a module. |
| [minesweeper](https://github.com/wippy-windows/minesweeper) | `windows/minesweeper` | Minesweeper as Windows 95 had it: three levels, flags and Flag mode, chording, the smiley and the timer. The rules are a pure library with tests; the window is a declarative SDK application. |
| [weather](https://github.com/wippy-windows/weather) | `windows/weather` | A weather window with a city search, the temperature in the tray next to the clock, and a desktop widget. Data from Open-Meteo, no key needed. |
| [aicq](https://github.com/wippy-windows/aicq) | `windows/aicq` | aICQ: people and AI agents in one ICQ-style contact list, messages between people, agent dialogs, and a tray item next to the clock. |
| [app](https://github.com/wippy-windows/app) | — | The application that runs on a server: the kickside platform plus the desktop, the shell, the games and the apps from the Hub, with the logon, the SSH host (`ssh -p 2222`, one desktop per connection) and the system windows. Start here to run it. |
| [calculator](https://github.com/wippy-windows/calculator) | `windows/calculator` | The Windows 95 Calculator: the engine as a pure library, the window on the shell SDK. |
| [network](https://github.com/wippy-windows/network) | `windows/network` | Network Neighborhood: the runtime's cluster as computers on a network, with roles and the leader. |
| [antibug](https://github.com/wippy-windows/antibug) | `windows/antibug` | AntiBug: a test scanner in the style of a 1995 antivirus — the application's tests and the targets it declares, findings are failed tests. Administrators only. |
| [appwiz](https://github.com/wippy-windows/appwiz) | `windows/appwiz` | Add/Remove Programs: the application's wippy modules, installed and removed from the Start menu. Administrators only. |
| [regedit](https://github.com/wippy-windows/regedit) | `windows/regedit` | The Registry Editor: the runtime's registry as a tree, read-only. Administrators only. |
| [datetime](https://github.com/wippy-windows/datetime) | `windows/datetime` | Date/Time: the calendar, the analog clock and the time zone, opened by the taskbar clock. |
| [taskman](https://github.com/wippy-windows/taskman) | `windows/taskman` | Task Manager: applications, processes, performance and the node on four tabs, End Task and End Process. Administrators only. |
| [run](https://github.com/wippy-windows/run) | `windows/run` | Run…: a command line that opens in its own Bash window, at the root of the Start menu. |
| [module-template](https://github.com/wippy-windows/module-template) | `windows/module-template` | The template for a new module: a sample window with tests, the harness against the Hub, the SDK guide and the agent skill. "Use this template", `make init`, write your window. |
| [runtime](https://github.com/wippy-windows/runtime) | — | A fork of [wippyai/runtime](https://github.com/wippyai/runtime) carrying what the desktop needs before it lands upstream: the `gfx` module (pixels over Kitty/Sixel), the `terminal.ssh` host (one desktop per SSH connection) and terminal fixes. Work lives on the `wippy-projects` branch; its [releases](https://github.com/wippy-windows/runtime/releases) are built by CI for Linux, macOS and Windows. |

Modules are published to the [Wippy Hub](https://hub.wippy.ai) under the
organization `windows`, all public.

## Running it

The pixel chrome needs the runtime's `gfx` module, which is not in a Wippy
release yet; until it is, the shell runs on a build of the
[runtime fork](https://github.com/wippy-windows/runtime) (`wippy-projects`
branch).
The mechanics (`tui-desktop`) run on a released runtime.

The shell ships the original Windows 95 icon set from `shell32.dll`, which is
Microsoft's artwork; the code is MIT.

## Writing a window

A window is a Lua table with `init`, `view` and `update`; `view` returns a
tree of components (menu, list, table, tree, editor, tabs, buttons, gauges)
and the shell draws it in cells or pixels. A registry entry puts it in the
Start menu; a widget is the same shape with an `interval` and no input.
See the SDK guide in the shell repository.
