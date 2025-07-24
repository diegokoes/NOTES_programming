# hyprland > troubleshooting

## Summary
> [!summary]
> Common Hyprland issues and their solutions for smooth operation

## Theory

### Common Issues and Solutions

#### **Hyprland Won't Start**

**Symptoms:** Black screen, returns to TTY, or login manager
**Solutions:**
```bash
# Check if Wayland session is available
echo $XDG_SESSION_TYPE

# Verify installation
which Hyprland

# Check dependencies
ldd $(which Hyprland)

# Start with debug output
Hyprland -l debug

# Check logs
journalctl -u your-display-manager
```

#### **Applications Won't Launch**

**Symptoms:** Nothing happens when trying to open apps
**Solutions:**
```bash
# Set environment variables
export QT_QPA_PLATFORM=wayland
export GDK_BACKEND=wayland
export MOZ_ENABLE_WAYLAND=1

# Install XDG desktop portal
pacman -S xdg-desktop-portal-hyprland

# For X11 apps, ensure XWayland is available
pacman -S xorg-xwayland
```

#### **Screen Tearing or Performance Issues**

**Solutions:**
```bash
# In hyprland.conf
misc {
    force_default_wallpaper = 0
    disable_hyprland_logo = true
    vfr = true
    vrr = 1
}

render {
    explicit_sync = 2
    explicit_sync_kms = 2
}
```

#### **Monitor Configuration Issues**

**Symptoms:** Wrong resolution, duplicated displays
**Solutions:**
```bash
# List available monitors
hyprctl monitors

# Configure in hyprland.conf
monitor = DP-1,1920x1080@144,0x0,1
monitor = HDMI-A-1,1920x1080@60,1920x0,1

# Disable monitor
monitor = HDMI-A-1,disable

# Auto-configure
monitor = ,preferred,auto,1
```

#### **Audio Issues**

**Symptoms:** No sound, wrong audio device
**Solutions:**
```bash
# Install audio server
pacman -S pipewire pipewire-pulse pipewire-alsa

# Check audio devices
wpctl status

# Set default device
wpctl set-default DEVICE_ID

# Test audio
speaker-test -c 2
```

#### **Keyboard/Input Not Working**

**Solutions:**
```bash
# In hyprland.conf
input {
    kb_layout = us
    kb_options = caps:escape
    repeat_rate = 25
    repeat_delay = 600
}

# For special keys
device:epic-mouse-v1 {
    sensitivity = -0.5
}
```

#### **NVIDIA GPU Issues**

**Solutions:**
```bash
# Enable NVIDIA Wayland support
export GBM_BACKEND=nvidia-drm
export __GLX_VENDOR_LIBRARY_NAME=nvidia
export WLR_NO_HARDWARE_CURSORS=1

# In hyprland.conf
env = LIBVA_DRIVER_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
env = GBM_BACKEND,nvidia-drm
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = WLR_NO_HARDWARE_CURSORS,1

cursor {
    no_hardware_cursors = true
}
```

### Debug Tools and Commands

#### **Information Gathering**
```bash
# Get system info
hyprctl systeminfo

# Monitor information
hyprctl monitors all

# Active window details
hyprctl activewindow

# All windows
hyprctl clients

# Current configuration
hyprctl getoption general:layout

# Version information
Hyprland --version
```

#### **Performance Monitoring**
```bash
# FPS counter
hyprctl keyword debug:overlay true

# Performance info
hyprctl keyword debug:damage_tracking 2

# Memory usage
ps aux | grep Hyprland
```

#### **Log Analysis**
```bash
# Start with verbose logging
Hyprland -l debug 2>&1 | tee hyprland.log

# System logs
journalctl -f | grep -i hyprland

# Crash logs
coredumpctl list
coredumpctl gdb hyprland
```

### Configuration Validation

#### **Syntax Checking**
```bash
# Test configuration
hyprctl reload

# Check for errors
hyprctl getoption general:layout | head -1
```

#### **Backup Strategy**
```bash
# Backup working config
cp ~/.config/hypr/hyprland.conf ~/.config/hypr/hyprland.conf.backup

# Minimal working config
cat > ~/.config/hypr/hyprland-minimal.conf << EOF
monitor=,preferred,auto,1
exec-once=kitty
bind=SUPER,Return,exec,kitty
bind=SUPER SHIFT,Q,killactive
EOF
```

### Performance Optimization

#### **Reduce Resource Usage**
```bash
# In hyprland.conf
decoration {
    blur {
        enabled = false
    }
    drop_shadow = false
}

animations {
    enabled = false
}

misc {
    vfr = true
    vrr = 1
}
```

#### **GPU Optimization**
```bash
render {
    explicit_sync = 2
    explicit_sync_kms = 2
    direct_scanout = true
}
```

## Questions

> [!tip]- Hyprland is using too much CPU/RAM
> Disable animations and blur effects, enable VFR, and check for problematic plugins

> [!warning]- My screen is flickering or tearing
> Try enabling explicit sync, adjusting VRR settings, or disabling hardware cursors for NVIDIA

> [!danger]- Hyprland crashed and I can't recover
> Switch to TTY (Ctrl+Alt+F2), restore a backup config, or use a minimal configuration to get back to a working state

- - -
#hyprland
