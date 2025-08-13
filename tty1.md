# TL;DR: BIG FONT ONLY ON TTY1 (ARCH/CACHYOS)

Goal: Use a large Terminus console font (ter-132b) only on tty1 without affecting other virtual consoles or graphical terminal emulators.

## STEPS

1. Install font package (if not already):
   ```bash
   sudo pacman -S --needed terminus-font
   ```

2. Test font manually on a real VT (tty1):
   ```bash
   sudo setfont ter-132b
   ```

3. Create/modify systemd drop-in for getty@tty1:
   ```bash
   sudo systemctl edit getty@tty1.service
   ```
   Paste:
   ```
   [Service]
   ExecStartPre=
   ExecStartPre=/usr/bin/setfont ter-132b
   ```

4. Apply changes:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart getty@tty1
   ```

5. Switch to tty1 (Ctrl+Alt+F1 / F2 depending on layout) to see large font.

## REVERT

```bash
sudo systemctl revert getty@tty1.service
sudo systemctl restart getty@tty1
```

## NOTES

- Only affects tty1 from the login prompt onward (not very-early boot).
- Does not change fonts inside Wayland/Hyprland terminal emulators.
- Change to another size by editing the override and swapping ter-132b with e.g. ter-132n, ter-128b, ter-124n, etc.
