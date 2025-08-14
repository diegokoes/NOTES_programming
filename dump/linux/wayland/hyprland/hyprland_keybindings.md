# HYPRLAND > KEYBINDINGS

## SUMMARY
> [!summary]
> Complete guide to Hyprland keybindings configuration and common shortcuts

## THEORY

### KEYBINDING SYNTAX
```bash
# Basic syntax
bind = MODIFIERS, KEY, DISPATCHER, PARAMS

# Examples
bind = SUPER, Return, exec, kitty
bind = SUPER SHIFT, Q, killactive
bind = SUPER, F, fullscreen
```

### COMMON MODIFIERS
- `SUPER` - Windows/Super key
- `SHIFT` - Shift key
- `CTRL` - Control key
- `ALT` - Alt key
- `META` - Meta key

### ESSENTIAL KEYBINDINGS
```bash
# Variables
$mod = SUPER
$terminal = kitty
$fileManager = dolphin
$menu = wofi --show drun

# Application shortcuts
bind = $mod, Return, exec, $terminal
bind = $mod, E, exec, $fileManager
bind = $mod, R, exec, $menu
bind = $mod, B, exec, firefox

# Window management
bind = $mod SHIFT, Q, killactive
bind = $mod, F, fullscreen
bind = $mod, V, togglefloating
bind = $mod, P, pseudo
bind = $mod, J, togglesplit

# Focus movement
bind = $mod, left, movefocus, l
bind = $mod, right, movefocus, r
bind = $mod, up, movefocus, u
bind = $mod, down, movefocus, d

# Alternative vim-like focus
bind = $mod, h, movefocus, l
bind = $mod, l, movefocus, r
bind = $mod, k, movefocus, u
bind = $mod, j, movefocus, d

# Window movement
bind = $mod SHIFT, left, movewindow, l
bind = $mod SHIFT, right, movewindow, r
bind = $mod SHIFT, up, movewindow, u
bind = $mod SHIFT, down, movewindow, d

# Workspace switching
bind = $mod, 1, workspace, 1
bind = $mod, 2, workspace, 2
bind = $mod, 3, workspace, 3
bind = $mod, 4, workspace, 4
bind = $mod, 5, workspace, 5
bind = $mod, 6, workspace, 6
bind = $mod, 7, workspace, 7
bind = $mod, 8, workspace, 8
bind = $mod, 9, workspace, 9
bind = $mod, 0, workspace, 10

# Move windows to workspace
bind = $mod SHIFT, 1, movetoworkspace, 1
bind = $mod SHIFT, 2, movetoworkspace, 2
bind = $mod SHIFT, 3, movetoworkspace, 3
bind = $mod SHIFT, 4, movetoworkspace, 4
bind = $mod SHIFT, 5, movetoworkspace, 5
bind = $mod SHIFT, 6, movetoworkspace, 6
bind = $mod SHIFT, 7, movetoworkspace, 7
bind = $mod SHIFT, 8, movetoworkspace, 8
bind = $mod SHIFT, 9, movetoworkspace, 9
bind = $mod SHIFT, 0, movetoworkspace, 10

# Special workspaces (scratchpad)
bind = $mod, S, togglespecialworkspace, magic
bind = $mod SHIFT, S, movetoworkspace, special:magic

# Mouse bindings
bindm = $mod, mouse:272, movewindow
bindm = $mod, mouse:273, resizewindow

# Resize mode
bind = $mod, R, submap, resize
submap = resize
binde = , right, resizeactive, 10 0
binde = , left, resizeactive, -10 0
binde = , up, resizeactive, 0 -10
binde = , down, resizeactive, 0 10
bind = , escape, submap, reset
submap = reset
```

### MEDIA AND SYSTEM CONTROLS
```bash
# Volume controls
bind = , XF86AudioRaiseVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+
bind = , XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
bind = , XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle

# Brightness controls
bind = , XF86MonBrightnessUp, exec, brightnessctl set 10%+
bind = , XF86MonBrightnessDown, exec, brightnessctl set 10%-

# Media controls
bind = , XF86AudioPlay, exec, playerctl play-pause
bind = , XF86AudioNext, exec, playerctl next
bind = , XF86AudioPrev, exec, playerctl previous

# Screenshot
bind = , Print, exec, grim -g "$(slurp)" - | wl-copy
bind = $mod, Print, exec, grim ~/Pictures/screenshot-$(date +%Y%m%d-%H%M%S).png

# System controls
bind = $mod SHIFT, E, exec, wlogout
bind = $mod SHIFT, L, exec, swaylock
```

### ADVANCED BINDINGS
```bash
# Window groups
bind = $mod, G, togglegroup
bind = $mod, TAB, changegroupactive

# Layout switching
bind = $mod CTRL, Space, exec, hyprctl keyword general:layout "$(if [ "$(hyprctl getoption general:layout | grep -o 'dwindle\|master')" = "dwindle" ]; then echo master; else echo dwindle; fi)"

# Workspace cycling
bind = $mod, mouse_down, workspace, e+1
bind = $mod, mouse_up, workspace, e-1

# Global shortcuts (work in all contexts)
bind = $mod CTRL SHIFT, R, exec, hyprctl reload
```

## QUESTIONS

> [!tip]- How do I create custom keybindings?
> Add `bind = MODIFIERS, KEY, DISPATCHER, PARAMS` lines to your hyprland.conf file

> [!warning]- My keybindings aren't working
> Check for conflicts with existing bindings and ensure proper syntax. Use `hyprctl reload` after changes.

> [!danger]- I bound something wrong and can't use Hyprland
> Switch to a TTY (Ctrl+Alt+F2) and edit your config file, or delete problematic lines

- - -

