# HYPRLAND

## SUMMARY
> [!summary]
> Hyprland is a highly customizable dynamic tiling Wayland compositor with advanced features like animations, rounded corners, and extensive configuration options.

## THEORY

### WHAT IS HYPRLAND?
Hyprland is a dynamic tiling Wayland compositor written in C++. It features extensive customization, smooth animations, and modern Wayland protocol support.

### KEY FEATURES
- **Dynamic Tiling** - Automatic window arrangement
- **Animations** - Smooth window transitions and effects
- **Rounded Corners** - Modern aesthetic features
- **Multi-monitor Support** - Advanced display management
- **Plugin System** - Extensibility through plugins
- **IPC Support** - Control via `hyprctl` command

### INSTALLATION
```bash
# Arch Linux
pacman -S hyprland

# From source
git clone --recursive https://github.com/hyprwm/Hyprland
cd Hyprland
make all
```

### BASIC CONFIGURATION
Configuration file: `~/.config/hypr/hyprland.conf`

### ESSENTIAL COMMANDS
```bash
# Reload configuration
hyprctl reload

# Get active window info
hyprctl activewindow

# List all windows
hyprctl clients

# Control workspaces
hyprctl dispatch workspace 1

# Get monitors info
hyprctl monitors
```

### RELATED TOPICS
- [Configuration](hyprland_config.md)
- [Keybindings](hyprland_keybindings.md)
- [Plugins](hyprland_plugins.md)
- [Troubleshooting](hyprland_troubleshooting.md)

## QUESTIONS

> [!tip]- How do I reload Hyprland configuration?
> Use `hyprctl reload` to reload the configuration without restarting

> [!warning]- Why are my applications not starting?
> Make sure you have the required environment variables set and XDG portals installed

> [!danger]- Hyprland crashed, how do I recover?
> Switch to a TTY (Ctrl+Alt+F2) and restart your display manager or run `Hyprland` again

- - -
