# WAYLAND

## SUMMARY
> [!summary]
> Wayland is a modern display server protocol designed to replace X11, offering better security, performance, and graphics capabilities.

## THEORY

### WHAT IS WAYLAND?
Wayland is a display server protocol and a library implementing that protocol. It's designed as a replacement for the X Window System (X11) with improved security, simplicity, and performance.

### KEY ADVANTAGES OVER X11
- **Better Security** - Applications can't spy on each other or inject events
- **Improved Performance** - Direct rendering, reduced latency
- **Modern Graphics** - Better support for modern GPU features
- **Simpler Architecture** - Less complex than X11
- **Better Multi-monitor Support** - Native support for different DPI and refresh rates

### ARCHITECTURE
- **Compositor** - Combines window management and display server roles
- **Clients** - Applications that connect to the compositor
- **Protocols** - Extensions for specific functionality

### POPULAR WAYLAND COMPOSITORS
- **[Hyprland](hyprland.md)** - Feature-rich tiling compositor
- **Sway** - i3-compatible tiling compositor
- **GNOME (Mutter)** - GNOME's compositor
- **KDE (KWin)** - KDE's compositor
- **wlroots-based** - River, Wayfire, etc.

### ENVIRONMENT VARIABLES
```bash
# Force applications to use Wayland
export QT_QPA_PLATFORM=wayland
export GDK_BACKEND=wayland
export MOZ_ENABLE_WAYLAND=1
export CLUTTER_BACKEND=wayland
```

### COMMON TOOLS
- **[Wayland Tools](wayland_tools.md)** - Comprehensive tools guide
- **wlr-randr** - Display configuration
- **grim** - Screenshot utility
- **slurp** - Screen area selection
- **wl-clipboard** - Clipboard utilities
- **wofi/rofi** - Application launchers

## QUESTIONS

> [!tip]- How do I know if I'm running Wayland?
> Check the `$XDG_SESSION_TYPE` environment variable or run `echo $WAYLAND_DISPLAY`

> [!warning]- Are all applications compatible with Wayland?
> Most modern applications work well. Legacy X11 apps run through XWayland compatibility layer.

> [!danger]- Should I switch from X11 to Wayland?
> Wayland is generally recommended for new setups, but X11 might be better for older hardware or specific use cases requiring X11-only features.

- - -
