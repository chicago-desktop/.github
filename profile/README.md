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

## Repositories

| Repository | Hub module | What it is |
|---|---|---|
| [tui-desktop](https://github.com/chicago-desktop/tui-desktop) | `chicago/tui-desktop` | The mechanics: a compositor of overlapping windows for the terminal, real programs under a PTY, one desktop per SSH connection, a command channel an agent can drive. No look of its own — a theme brings it. |
| [shell](https://github.com/chicago-desktop/shell) | `chicago/shell` | The desktop shell on top: the pixel chrome, the Start menu, the logon screen, a declarative window SDK, Notepad, My Computer with one window per folder, Display Properties and desktop widgets. Everything else is a module. |
| [app](https://github.com/chicago-desktop/app) | — | The application that runs on a server: the kickside platform plus the desktop base, the shell and the modules from the Hub, with the logon, the SSH host (`ssh -p 2222`, one desktop per connection) and the system windows. Start here to run it. |
| [module-template](https://github.com/chicago-desktop/module-template) | `chicago/module-template` | The template for a new module: a sample window with tests, the harness against the Hub, the SDK guide and the agent skill. "Use this template", `make init`, write your window. |
| [minesweeper](https://github.com/chicago-desktop/minesweeper) | `chicago/minesweeper` | Minesweeper: three levels, flags and Flag mode, chording, the smiley and the timer. The rules are a pure library with tests; the window is a declarative SDK application. |
| [weather](https://github.com/chicago-desktop/weather) | `chicago/weather` | A weather window with a city search, the temperature in the tray next to the clock, and a desktop widget. Data from Open-Meteo, no key needed. |
| [aicq](https://github.com/chicago-desktop/aicq) | `chicago/aicq` | aICQ: people and AI agents in one ICQ-style contact list, messages between people, agent dialogs, and a tray item next to the clock. |
| [calculator](https://github.com/chicago-desktop/calculator) | `chicago/calculator` | Calculator: the engine as a pure library, the window on the shell SDK. |
| [network](https://github.com/chicago-desktop/network) | `chicago/network` | Network Neighborhood: the runtime's cluster as computers on a network, with roles and the leader. |
| [antibug](https://github.com/chicago-desktop/antibug) | `chicago/antibug` | AntiBug: a test scanner in the style of a 1995 antivirus — the application's tests and the targets it declares, findings are failed tests. Administrators only. |
| [appwiz](https://github.com/chicago-desktop/appwiz) | `chicago/appwiz` | Add/Remove Programs: the application's Wippy modules, installed and removed from the Start menu. Administrators only. |
| [regedit](https://github.com/chicago-desktop/regedit) | `chicago/regedit` | Registry Editor: the runtime's registry as a tree, read-only. Administrators only. |
| [datetime](https://github.com/chicago-desktop/datetime) | `chicago/datetime` | Date/Time: the calendar, the analog clock and the time zone, opened by the taskbar clock. |
| [taskman](https://github.com/chicago-desktop/taskman) | `chicago/taskman` | Task Manager: applications, processes, performance and the node on four tabs, End Task and End Process. Administrators only. |
| [run](https://github.com/chicago-desktop/run) | `chicago/run` | Run…: a command line that opens in its own Bash window, at the root of the Start menu. |
| [runtime](https://github.com/chicago-desktop/runtime) | — | A fork of [wippyai/runtime](https://github.com/wippyai/runtime) carrying what the desktop needs before it lands upstream: the `gfx` module (pixels over Kitty/Sixel), the `terminal.ssh` host (one desktop per SSH connection) and terminal fixes. Work lives on the `wippy-projects` branch; its [releases](https://github.com/chicago-desktop/runtime/releases) are built by CI for Linux, macOS and the other major platforms. |

Modules are published to the [Wippy Hub](https://hub.wippy.ai) under the
organization `chicago`, all public.

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
