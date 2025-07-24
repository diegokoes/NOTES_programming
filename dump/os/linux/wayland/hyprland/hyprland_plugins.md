# hyprland > plugins

## Summary
> [!summary]
> Guide to Hyprland plugins for extending functionality and customization

## Theory

### Plugin System
Hyprland supports plugins written in C++ that can extend its functionality. Plugins are loaded dynamically at runtime.

### Installing Plugins
```bash
# Using hyprpm (Hyprland Plugin Manager)
hyprpm add https://github.com/author/plugin-name
hyprpm enable plugin-name

# Manual installation
git clone https://github.com/author/plugin-name
cd plugin-name
make all
# Copy .so file to plugins directory
```

### Popular Plugins

#### **hyprland-plugins (Official Collection)**
```bash
# Borders++
hyprpm add https://github.com/hyprwm/hyprland-plugins
hyprpm enable borders-plus-plus
```
- **Borders++** - Enhanced border management
- **csgo-vulkan-fix** - CS:GO Vulkan compatibility
- **hyprbars** - Window title bars

#### **hypr-dynamic-cursors**
```bash
hyprpm add https://github.com/VirtCode/hypr-dynamic-cursors
```
- Dynamic cursor animations and effects

#### **hyprfocus**
```bash
hyprpm add https://github.com/pyt0xic/hyprfocus
```
- Advanced window focus animations

#### **hyprwinwrap**
```bash
hyprpm add https://github.com/hyprwm/hyprwinwrap
```
- Animated wallpapers support

#### **hyprsplit**
```bash
hyprpm add https://github.com/shezdy/hyprsplit
```
- Enhanced window splitting

### Plugin Configuration
Add plugin settings to your `hyprland.conf`:

```bash
# Enable plugins
plugin = /path/to/plugin.so

# Plugin-specific settings
plugin {
    borders-plus-plus {
        add_borders = 1
        col.border_1 = rgb(ffffff)
        col.border_2 = rgb(2222ff)
        border_size_1 = 10
        border_size_2 = -1
        natural_rounding = yes
    }
    
    hyprbars {
        bar_color = rgb(2222ff)
        bar_height = 28
        bar_text_size = 11
        bar_text_font = Ubuntu Nerd Font
        bar_title_enabled = true
        bar_buttons_alignment = right
        bar_part_of_window = true
        bar_precedence_over_border = true
    }
}
```

### Plugin Management Commands
```bash
# List available plugins
hyprpm list

# Update plugins
hyprpm update

# Remove plugin
hyprpm remove plugin-name

# Reload plugins
hyprctl plugin reload plugin-name

# List loaded plugins
hyprctl plugin list
```

### Developing Plugins

#### Basic Plugin Structure
```cpp
#include <hyprland/src/plugins/PluginAPI.hpp>

APICALL EXPORT std::string PLUGIN_API_VERSION() {
    return HYPRLAND_API_VERSION;
}

APICALL EXPORT PLUGIN_DESCRIPTION_INFO PLUGIN_INIT(HANDLE handle) {
    PHANDLE = handle;
    
    // Plugin initialization code
    
    return {"PluginName", "Description", "Author", "1.0"};
}

APICALL EXPORT void PLUGIN_EXIT() {
    // Cleanup code
}
```

#### Building Plugins
```bash
# Makefile example
CC = g++
CFLAGS = -shared -fPIC -O3 -std=c++23
INCLUDES = -I/usr/include/hyprland

all:
	$(CC) $(CFLAGS) $(INCLUDES) plugin.cpp -o plugin.so

install:
	cp plugin.so ~/.local/share/hyprland/plugins/
```

### Troubleshooting Plugins

#### Common Issues
- **Plugin won't load** - Check API version compatibility
- **Crashes** - Disable plugins one by one to identify problematic ones
- **Performance issues** - Some plugins may impact performance

#### Debug Commands
```bash
# Check plugin errors
hyprctl plugin list | grep -i error

# Reload all plugins
hyprctl reload

# Disable all plugins temporarily
hyprctl keyword plugin ""
```

## Questions

> [!tip]- How do I find available plugins?
> Check the Hyprland community, GitHub, or the official plugins repository

> [!warning]- Are plugins safe to use?
> Only install plugins from trusted sources. Plugins have full system access and can potentially cause security issues.

> [!danger]- A plugin crashed Hyprland, what do I do?
> Remove the problematic plugin from your config and restart Hyprland. You can disable plugins with `hyprctl keyword plugin ""`

- - -
#hyprland
