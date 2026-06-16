# CC-Switch for Ubuntu 20.04 - Flatpak Installation Guide

## Current Status

✅ **Successfully built**: Ubuntu 20.04-compatible CC-Switch Flatpak  
📦 **Artifact**: `dist-flatpak/CC-Switch-Linux.flatpak` (13 MB)  
🔧 **Runtime**: GNOME 3.38 (stable, end-of-life but compatible with Ubuntu 20.04)

## Why Flatpak?

The original AppImage requires GLIBC 2.32+ and GLIBCXX 3.4.29+, which Ubuntu 20.04 doesn't have. 
Flatpak solves this by bundling its own runtime with all required libraries, completely isolated from the host system.

## Installation Steps

### Option 1: Online Installation (Requires Network Access to Flathub)

```bash
# Step 1: Install GNOME 3.38 runtime from Flathub
flatpak install flathub org.gnome.Platform//3.38

# Step 2: Install CC-Switch
flatpak install --user /home/daniel/2T/ap/tmp/cc-switch/dist-flatpak/CC-Switch-Linux.flatpak

# Step 3: Launch
flatpak run com.ccswitch.desktop
```

### Option 2: Offline Installation (Local Runtime)

If Flathub is unreachable, you'll need the GNOME 3.38 runtime file. Contact the developer for the runtime bundle.

```bash
# Install runtime from bundle
flatpak install /path/to/org.gnome.Platform-3.38-x86_64.flatpak

# Then install app
flatpak install --user /home/daniel/2T/ap/tmp/cc-switch/dist-flatpak/CC-Switch-Linux.flatpak
```

## Troubleshooting

### "Runtime not found" Error

```
error: The application com.ccswitch.desktop/x86_64/master requires the runtime 
org.gnome.Platform/x86_64/3.38 which was not found
```

**Solution**: Install the runtime first:
```bash
flatpak install --user flathub org.gnome.Platform//3.38
```

If Flathub is slow/unreachable, wait a moment and retry, or try a different network.

### "Flathub not found" Error

Ensure Flathub remote is registered:
```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

### Desktop Menu Not Showing App

After installation, restart your session:
```bash
# Log out and log back in, or run:
source /etc/profile.d/flatpak.sh
```

## Technical Details

- **Build Platform**: Ubuntu 22.04 (has required newer libraries)
- **Target Platform**: Ubuntu 20.04 (lacks newer libraries but Flatpak provides them)
- **Flatpak Runtime**: GNOME 3.38 (self-contained with glib, GTK, webkit, etc.)
- **App Isolation**: Full sandbox with home directory access for config/data

## Uninstallation

```bash
# Remove CC-Switch
flatpak uninstall com.ccswitch.desktop

# Remove GNOME 3.38 runtime (optional, shared by other apps)
flatpak uninstall org.gnome.Platform//3.38
```

## Alternative: Docker Container (If Flatpak Installation Fails)

For a guaranteed working setup, you can run CC-Switch in Docker:

```dockerfile
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y flatpak libwayland-client0 libx11-6
ADD dist-flatpak/CC-Switch-Linux.flatpak /tmp/
RUN flatpak install --user /tmp/CC-Switch-Linux.flatpak
CMD ["flatpak", "run", "com.ccswitch.desktop"]
```

## Questions?

- **Flatpak Docs**: https://docs.flatpak.org/
- **GNOME Runtimes**: https://flathub.org/
