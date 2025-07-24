# hyprland > config

## Summary
> [!summary]
> Comprehensive guide to configuring Hyprland with examples for common settings

## Theory

### Configuration File Location
Main config: `~/.config/hypr/hyprland.conf`

### Basic Structure
```bash
# Comments start with #
# Variables
$mod = SUPER

# Monitors
monitor = ,preferred,auto,1

# Input settings
input {
    kb_layout = us
    follow_mouse = 1
    touchpad {
        natural_scroll = false
    }
    sensitivity = 0
}

# General settings
general {
    gaps_in = 5
    gaps_out = 20
    border_size = 2
    col.active_border = rgba(33ccffee) rgba(00ff99ee) 45deg
    col.inactive_border = rgba(595959aa)
    layout = dwindle
}

# Decorations
decoration {
    rounding = 10
    blur {
        enabled = true
        size = 3
        passes = 1
    }
    drop_shadow = true
    shadow_range = 4
    shadow_render_power = 3
    col.shadow = rgba(1a1a1aee)
}

# Animations
animations {
    enabled = true
    bezier = myBezier, 0.05, 0.9, 0.1, 1.05
    animation = windows, 1, 7, myBezier
    animation = windowsOut, 1, 7, default, popin 80%
    animation = border, 1, 10, default
    animation = borderangle, 1, 8, default
    animation = fade, 1, 7, default
    animation = workspaces, 1, 6, default
}

# Window rules
windowrulev2 = float,class:^(kitty)$,title:^(kitty)$
windowrulev2 = opacity 0.8 0.8,class:^(kitty)$
```

### Monitor Configuration
```bash
# Single monitor
monitor = DP-1,1920x1080@60,0x0,1

# Multiple monitors
monitor = DP-1,1920x1080@144,0x0,1
monitor = HDMI-A-1,1920x1080@60,1920x0,1

# Auto-detect
monitor = ,preferred,auto,1
```

### Input Configuration
```bash
input {
    kb_layout = us,es
    kb_variant = 
    kb_model =
    kb_options = grp:alt_shift_toggle
    kb_rules =
    
    follow_mouse = 1
    
    touchpad {
        natural_scroll = false
        disable_while_typing = true
        tap-to-click = true
    }
    
    sensitivity = 0 # -1.0 - 1.0, 0 means no modification
    accel_profile = flat
}
```

### Workspace Rules
```bash
# Bind workspaces to monitors
workspace = 1, monitor:DP-1
workspace = 2, monitor:DP-1
workspace = 10, monitor:HDMI-A-1

# Default workspace for applications
windowrulev2 = workspace 2,class:^(firefox)$
windowrulev2 = workspace 3,class:^(code)$
```

### Environment Variables
```bash
env = XCURSOR_SIZE,24
env = QT_QPA_PLATFORM,wayland
env = GDK_BACKEND,wayland
env = MOZ_ENABLE_WAYLAND,1
```

## Questions

> [!tip]- Where should I put my Hyprland configuration?
> Place it in `~/.config/hypr/hyprland.conf` or create modular configs in the same directory

> [!warning]- Why aren't my changes taking effect?
> Run `hyprctl reload` after making changes, or check syntax with `hyprctl reload` for error messages

> [!danger]- My Hyprland won't start after configuration changes
> Check the syntax in your config file, particularly brackets and semicolons. Use a backup config if needed.

- - -
#hyprland
