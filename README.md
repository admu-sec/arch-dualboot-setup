# Arch Linux Setup

![Arch Linux](https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&logo=arch-linux&logoColor=white)
![Hyprland](https://img.shields.io/badge/Hyprland-58E1FF?style=for-the-badge&logoColor=black)
![NVIDIA](https://img.shields.io/badge/NVIDIA-76B900?style=for-the-badge&logo=nvidia&logoColor=white)

Arch Linux installation with Hyprland as window manager, JaKooLit dotfiles and dual-boot with Windows via GRUB.

---

## System

| | |
|---|---|
| **OS** | Arch Linux x86_64 |
| **Shell** | zsh + oh-my-zsh |
| **Boot** | Dual-boot Windows/Arch via GRUB |

---

## Hardware

| | |
|---|---|
| **CPU** | Intel Core i5-9600K |
| **GPU** | NVIDIA GeForce GTX 1660 Ti (nvidia-open driver) |
| **RAM** | 16 GB |
| **Primary Monitor** | 27" 2560x1440 @ 144Hz (DisplayPort) |
| **Secondary Monitor** | 22" 1920x1080 @ 60Hz (HDMI) |

---

## Desktop

| | |
|---|---|
| **Window Manager** | Hyprland (Wayland) |
| **Dotfiles** | [JaKooLit Arch-Hyprland](https://github.com/JaKooLit/Arch-Hyprland) |
| **Bar** | Waybar |
| **Terminal** | Kitty |
| **Launcher** | Rofi |
| **File Manager** | Thunar |
| **Wallpaper** | awww |
| **Lock Screen** | Hyprlock |
| **Display Manager** | SDDM |
| **Browser** | Firefox |

---

## Installed Applications

- Firefox
- Brave Browser
- Spotify
- Discord
- Steam
- Mullvad VPN
- Fastfetch

---

## Configuration

### Monitor Configuration (`~/.config/hypr/monitors.conf`)

monitor=DP-2,2560x1440@143.91,0x0,1
monitor=HDMI-A-1,1920x1080@60,2560x0,1

---

### Locale (`/etc/locale.gen`)

en_US.UTF-8 UTF-8

---

### Wallpaper Autostart (`~/.config/hypr/UserConfigs/Startup_Apps.conf`)

exec-once = awww-daemon --no-cache --format xrgb

---

## Keybindings

| Keybind | Function |
|---|---|
| `SUPER + Enter` | Terminal (Kitty) |
| `SUPER + R` | App launcher (Rofi) |
| `SUPER + W` | Change wallpaper |
| `SUPER + H` | Hyprland hints/keybindings |
| `SUPER + E` | File manager (Thunar) |
| `SUPER + B` | Browser (Firefox) |
| `SUPER + Q` | Close active window |
| `SUPER + SHIFT + R` | Reload Hyprland |
| `CTRL + ALT + L` | Lock screen (Hyprlock) |
| `SUPER + Print` | Screenshot region |

---

## Issues & Fixes

### swww → awww
After `pacman -Syu`, `swww` was replaced by `awww`, breaking wallpaper autostart and all wallpaper scripts. Fixed by updating `Startup_Apps.conf` and running `sed` to replace all `swww` references in JaKooLit scripts.

### Locale Issues
`en_US.UTF-8` was not generated, causing crashes in rofi, hyprlock and GTK apps with *"Failed to set locale"*. Fixed with:

```bash
sudo locale-gen
sudo localectl set-locale LANG=en_US.UTF-8
```

### Hyprlock Missing
Hyprlock was not included in JaKooLit's default installation. Dependency conflicts with `hyprutils` required a full system update before installation.

```bash
sudo pacman -Syu
sudo pacman -S hyprlock
```

### 144Hz Configuration
Hyprland was running the monitor at 60Hz despite supporting 144Hz. The correct file to edit is `~/.config/hypr/monitors.conf`. Syntax must be exact with no spaces.

### Rofi App Launcher
`SUPER+A` was already bound to the Overview function. Fixed with a custom binding on `SUPER+R` in `UserKeybinds.conf`.

### Mullvad VPN – PGP Key
Default keyserver was unreachable. Fixed by manually importing the key:

```bash
gpg --keyserver hkps://keyserver.ubuntu.com --recv-keys A1198702FC3E0A09A9AE5B75D5A1D4F266DE8DDF
```

### SDDM Background
`Backgrounds/default` was a file, not a folder. Fixed by copying the desired image and naming it `default`, then updating `theme.conf` with the correct path.

### GRUB Dual-Boot Clock
Windows and Linux had different clock times due to Windows using local time and Linux using UTC. Fixed with:

```bash
timedatectl set-local-rtc 0
```

And a Windows registry value for `RealTimeIsUniversal`.

---

## Useful Commands

```bash
# System
sudo pacman -Syu                            # Update system
sudo pacman -S <package>                    # Install package
yay -S <package>                            # Install AUR package
fastfetch                                   # Show system info
hyprctl monitors                            # Show monitor info
hyprctl dispatch exit                       # Log out from Hyprland

# Waybar
pkill waybar && waybar &disown              # Restart waybar

# Wallpaper
awww-daemon --no-cache &                    # Start wallpaper daemon
awww img ~/Pictures/wallpapers/image.png    # Change wallpaper

# Lock screen
hyprlock                                    # Lock screen manually
```


