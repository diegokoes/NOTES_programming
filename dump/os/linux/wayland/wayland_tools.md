# wayland > tools

## Summary
> [!summary]
> Essential tools and utilities for Wayland desktop environments and compositors

## Theory

### Core Wayland Tools

#### **Display and Screen Management**
- **wlr-randr** - Display configuration for wlroots-based compositors
- **kanshi** - Dynamic display configuration
- **wdisplays** - GUI display manager

```bash
# Install display tools
pacman -S wlr-randr kanshi wdisplays

# Configure displays
wlr-randr --output DP-1 --mode 1920x1080@144
```

#### **Screenshots and Screen Recording**
- **grim** - Screenshot utility
- **slurp** - Screen area selection
- **swappy** - Screenshot annotation
- **wf-recorder** - Screen recording

```bash
# Install screenshot tools
pacman -S grim slurp swappy wf-recorder

# Take screenshot
grim ~/screenshot.png

# Screenshot selection
grim -g "$(slurp)" ~/selection.png

# Copy to clipboard
grim -g "$(slurp)" - | wl-copy

# Record screen
wf-recorder -f ~/recording.mp4
```

#### **Clipboard Management**
- **wl-clipboard** - Command-line clipboard utilities
- **cliphist** - Clipboard history manager

```bash
# Install clipboard tools
pacman -S wl-clipboard cliphist

# Copy to clipboard
echo "text" | wl-copy

# Paste from clipboard
wl-paste

# Clipboard history
cliphist list | wofi --dmenu | cliphist decode | wl-copy
```

### Application Launchers

#### **wofi** - Wayland-native launcher
```bash
# Install
pacman -S wofi

# Basic usage
wofi --show drun

# Custom configuration
mkdir -p ~/.config/wofi
cat > ~/.config/wofi/config << EOF
width=600
height=300
location=center
show=drun
prompt=Search...
filter_rate=100
allow_markup=true
no_actions=true
halign=fill
orientation=vertical
content_halign=fill
insensitive=true
allow_images=true
image_size=40
gtk_dark=true
EOF
```

#### **rofi-wayland** - Rofi for Wayland
```bash
# Install
yay -S rofi-lbonn-wayland-git

# Usage
rofi -show drun
```

### Terminal Emulators

#### **Popular Wayland Terminals**
- **kitty** - GPU-accelerated terminal
- **alacritty** - Cross-platform terminal
- **foot** - Lightweight Wayland terminal
- **wezterm** - Feature-rich terminal

```bash
# Install terminals
pacman -S kitty alacritty foot wezterm

# Kitty with Wayland support
kitty --single-instance
```

### File Managers

#### **GUI File Managers**
- **nautilus** - GNOME file manager
- **dolphin** - KDE file manager
- **thunar** - XFCE file manager
- **nemo** - Cinnamon file manager

#### **Terminal File Managers**
- **[yazi](../yazi/yazi.md)** - Modern terminal file manager
- **ranger** - Vi-like file manager
- **lf** - Lightweight file manager

### Notification Systems

#### **mako** - Lightweight notification daemon
```bash
# Install
pacman -S mako

# Configuration ~/.config/mako/config
background-color=#2d2d2d
text-color=#ffffff
border-color=#5e81ac
border-size=2
border-radius=5
default-timeout=5000
```

#### **dunst** - Configurable notification daemon
```bash
# Install
pacman -S dunst

# Works with Wayland via XWayland
```

### Status Bars

#### **waybar** - Highly customizable status bar
```bash
# Install
pacman -S waybar

# Basic configuration
mkdir -p ~/.config/waybar
cat > ~/.config/waybar/config.jsonc << EOF
{
    "layer": "top",
    "position": "top",
    "height": 30,
    "modules-left": ["hyprland/workspaces"],
    "modules-center": ["clock"],
    "modules-right": ["pulseaudio", "network", "battery"],
    
    "clock": {
        "format": "{:%Y-%m-%d %H:%M}"
    },
    
    "pulseaudio": {
        "format": "{volume}% {icon}",
        "format-muted": "🔇"
    }
}
EOF
```

#### **eww** - Widget system
```bash
# Install
yay -S eww-wayland

# Highly customizable widgets
```

### Screen Lockers

#### **swaylock** - Screen locker for Wayland
```bash
# Install
pacman -S swaylock

# Basic usage
swaylock

# With effects
swaylock --screenshots --effect-blur 7x5
```

#### **hyprlock** - Hyprland's screen locker
```bash
# Install
pacman -S hyprlock

# Configuration in ~/.config/hypr/hyprlock.conf
```

### Wallpaper Tools

#### **swaybg** - Wallpaper utility
```bash
# Install
pacman -S swaybg

# Set wallpaper
swaybg -i ~/wallpaper.jpg

# Different modes
swaybg -i ~/wallpaper.jpg -m fill
```

#### **hyprpaper** - Hyprland wallpaper daemon
```bash
# Configuration in ~/.config/hypr/hyprpaper.conf
preload = ~/wallpaper.jpg
wallpaper = ,~/wallpaper.jpg
```

### Logout/Session Management

#### **wlogout** - Logout menu
```bash
# Install
pacman -S wlogout

# Usage
wlogout
```

### Audio Control

#### **pipewire** ecosystem
```bash
# Install
pacman -S pipewire pipewire-pulse pipewire-alsa wireplumber

# Audio control
wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+
wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
```

#### **pavucontrol** - GUI audio control
```bash
# Install
pacman -S pavucontrol

# Launch
pavucontrol
```

## Questions

> [!tip]- Which screenshot tool should I use?
> Use grim + slurp for basic screenshots, add swappy for annotation features

> [!warning]- Some GUI apps don't work properly
> Make sure you have XDG portals installed: `xdg-desktop-portal-wlr` or `xdg-desktop-portal-hyprland`

> [!danger]- X11 applications look blurry or don't scale properly
> Set proper scaling with `xrandr --dpi 96` or configure XWayland scaling in your compositor

- - -
#wayland
