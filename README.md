# libcursespp
libcursespp is a modern C++ TUI library based on Curses

<p align="center">
  <img src="/docs/resources/demo.png" width="75%">
</p>

## Installation
> [!IMPORTANT]
> This project depends on a Curses implementation library and the Meson build system.
> Most Unix users should have `ncurses` installed by default, but Windows users need to install PDCurses manually.

After cloning and entering the repository you can use Meson to compile this project:
```bash
meson setup builddir
cd builddir
meson compile
```

## Usage
### Window management
The main entry point of this library is the `CursesApp` class defined in the `CursesApp.hh` header file. It manages all of the windows available in your app and allows you to display them.

Example: creating a `CursesApp` and spawning a 10x10 window:

```cpp
CursesApp program;
program.addWindow(new GridWindow("My window", 10, 10, 0, 0));
```

You can then refer to the window by its name:

```cpp
auto& grid = static_cast<GridWindow&>(program.get("My window"));
```

### Available windows
All windows inherit from the `Window` class defined in the `Window.hh` file. Each window has a title, position and can listen to events. The library comes with a set of premade `Window` types:
- `Window` - an empty window
- `BorderedWindow` - a window with borders and a title
- `GridWindow` - a window that allows you to display a grid of items
- `ItemListWindow` - a window that displays a list of items
- `StatusBarWindow` - a window that can display a status bar
- `InputListenerWindow` - a virtual window for capturing events

### Input events
The `Window` class provides a thin abstraction over Curses input handling. Each listener window can listen for events by calling `Window::getChar()`. This function returns user input encoded in a char.

> [!WARNING]
> `Window::getChar()` is a blocking method.

Supported input types:
- Keyboard input
- Resize input (`KEY_RESIZE`)
- Mouse input (`KEY_MOUSE`)

### Main app loop
Putting everything together, a typical main app loop looks something like this:

```cpp
CursesApp program;
program.addWindows({
    new InputListenerWindow("Input"),
    // Add other app windows here
});

while (isRunning()) {
    // do things

    program.display();

    input = program.get("Input").getChar();
    // handle input
}
```

## Additional information
For a more detailed example on how this library can be used check out the `Simulator` class inside the [Organism Simulator](https://github.com/sajmon170/OrganismSim-CPP) git repository.
