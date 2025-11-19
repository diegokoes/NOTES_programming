# YAZI > MIME TYPES

The `[open]` section in yazi.toml defines **rules** that match files by MIME type or name pattern, then routes them to appropriate programs (openers).


## How MIME Type Matching Works

### Rule Matching Priority

Yazi evaluates rules **top-to-bottom** and uses the **first match**:

```toml
[open]
rules = [
  { mime = "text/*",        use = "edit-nano" },      # Matches first
  { mime = "inode/x-empty", use = "edit-nano" },
  { name = "*.json",        use = "edit-nano" },      # Falls back to name
]
```

**Matching criteria:**
- **`mime`** - MIME type (supports wildcards like `text/*`, `image/*`)
- **`name`** - Filename pattern (glob syntax: `*.extension`)
- **`regex`** (optional) - Regular expression pattern

### Example: Text Files

```toml
{ mime = "text/*",        use = "edit-nano" }
```

- Matches all MIME types starting with `text/` (e.g., `text/plain`, `text/html`, `text/css`)
- Routes to the `edit-nano` opener
- Opens with nano in blocking mode (waits for you to close the editor)

---

## Openers 

An opener is a **command definition** that specifies how to launch a program:

```toml
[opener]
edit-nano = [
  { run = 'nano "$@"', block = true, desc = "Edit with nano", for = "unix" },
]
```

### Opener Properties

| Property | Purpose |
|----------|---------|
| **`run`** | Command to execute (`$@` = selected files) |
| **`block`** | Wait for command to finish (true/false) |
| **`orphan`** | Run in background (detach from terminal) |
| **`for`** | OS filter: `"linux"`, `"unix"`, `"macos"`, `"windows"` |
| **`desc`** | Human-readable description |


## `open-xdg` Deep Dive

### What is `xdg-open`?

**`xdg-open`** is a **freedesktop.org standard** utility that opens files with your system's default applications.

```bash
xdg-open document.pdf      # Opens PDF with your default PDF viewer
xdg-open image.jpg         # Opens image with default image viewer
xdg-open website.html      # Opens HTML in default browser
```

### How It Works

1. Examines file MIME type (via `file` command or extension)
2. Checks your desktop environment's MIME type associations
3. Launches the configured default application
4. Returns immediately (orphan mode)

### Why Use `xdg-open`?

✅ **Respects user preferences** - Uses your system defaults  
✅ **Desktop-aware** - Works with GNOME, KDE, XFCE, etc.  
✅ **Flexible** - Handles unknown file types gracefully  
✅ **No hardcoding** - Doesn't force specific programs  
✅ **Fallback option** - Acts as safety net when other openers fail

### Your Usage

In your config, `open-xdg` serves as a **fallback**:

```toml
[open]
rules = [
  # Specific handlers
  { mime = "video/*",       use = [ "play-vlc", "open-xdg", "media-info" ] },
  { mime = "image/*",       use = [ "open-xdg", "exif" ] },
  
  # Ultimate fallback
  { name = "*",             use = "open-xdg" },
]
```

**Fallback order:**
1. Try `play-vlc` for videos
2. If VLC unavailable → try `open-xdg` (system default)
3. If that fails → try `media-info` (show file info)

---

## Complex Rule Example: Videos

```toml
{ mime = "video/*",       use = [ "play-vlc", "open-xdg", "media-info" ] },
{ name = "*.mp4",         use = [ "play-vlc", "open-xdg", "media-info" ] },
```

**What happens when you open `movie.mp4`:**

1. **First match wins** - `mime = "video/*"` matches first
2. **Try openers in order:**
   - Try `play-vlc` → If installed, opens in VLC (orphan)
   - Fail → Try `open-xdg` → If available, uses system default
   - Fail → Try `media-info` → Shows file metadata
3. **If all fail** → Error message shown



## Name Pattern Matching (Fallback)

When MIME type detection fails

```toml
{ name = "*.json",        use = "edit-nano" },
{ name = "*.html",        use = [ "open-xdg", "edit-nano" ] },
{ name = "*.tar.gz",      use = "extract" },
```


