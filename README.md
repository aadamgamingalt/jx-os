# JX-OS

A privacy-focused Linux OS built on Debian Bookworm with XFCE.  
Developed by Voidagon Studios.

---

## Threat model coverage

- **Data minimisation** - zero telemetry, hardened sysctl, stripped packages
- **ISP / surveillance** - Tor, dnscrypt-proxy (DoH), UFW strict firewall, WireGuard-ready
- **Anonymity** - WebRTC blocked, MAC randomisation, hostname randomisation, Firefox arkenfox hardening
- **Physical access** - USBGuard, AppArmor, auto screen lock, core dumps disabled, SSH off by default

---

## Building

### Via GitHub Actions (recommended)

Push to `main` and the ISO builds automatically. Download it from the **Actions** tab under the latest run's artifacts.

To create a release:

```bash
git tag v0.1.0
git push origin v0.1.0
```

This triggers a GitHub Release with the ISO attached.

### Locally (Debian/Ubuntu host required)

```bash
sudo apt install live-build debootstrap squashfs-tools xorriso librsvg2-bin imagemagick
git clone https://github.com/yourusername/jx-os
cd jx-os

# Copy your assets
cp your-assets/*.svg assets/

# Run the build
cd config
sudo lb build
```

---

## Repo structure

```
jx-os/
├── .github/workflows/build.yml       # CI pipeline
├── assets/                           # SVG wallpapers, logos
├── config/
│   ├── package-lists/
│   │   ├── base.list.chroot          # privacy + hardening packages
│   │   └── desktop.list.chroot       # XFCE + apps
│   ├── hooks/normal/
│   │   ├── 0001-harden.hook.chroot   # firewall, sysctl, kernel modules, USBGuard
│   │   ├── 0002-theme.hook.chroot    # GTK3/2 JX-OS dark theme
│   │   ├── 0003-xfce-desktop.hook.chroot  # XFCE panel, picom, terminal
│   │   ├── 0004-firefox.hook.chroot  # arkenfox-style Firefox hardening
│   │   ├── 0005-lightdm.hook.chroot  # login screen branding
│   │   ├── 0006-grub-theme.hook.chroot     # GRUB boot theme
│   │   ├── 0007-jxos-apps.hook.chroot      # About app, privacy checker
│   │   └── 0099-finalise.hook.chroot # cleanup + permission lock
│   └── includes.chroot/              # files dropped into ISO filesystem
│       ├── etc/                      # system config overrides
│       └── usr/share/
│           ├── backgrounds/jxos/     # wallpapers (PNG, converted from SVG)
│           ├── grub/themes/jxos/     # GRUB theme files
│           └── jxos/                 # logos, about.svg
```

---

## Adding your assets

Place these files in the `assets/` directory before pushing:

| File | Used for |
|------|----------|
| `wallpaperdefault.svg` | Default wallpaper + login screen |
| `wallpaper-midnight.svg` | Midnight theme wallpaper |
| `wallpaper-ember.svg` | Ember theme wallpaper |
| `wallpaper-forest.svg` | Forest theme wallpaper |
| `wallpaper-sunset.svg` | Sunset theme wallpaper |
| `logo-Logo.svg` | Standard logo |
| `logo-LogoBold.svg` | Bold logo - used for login screen + GRUB |
| `about.svg` | About screen reference asset |

---

## Default live session credentials

| Field | Value |
|-------|-------|
| Username | `jxos` |
| Password | `jxos` |

---

## Post-install recommended steps

1. Run `jxos-privacy-check` in terminal to verify all services are active
2. Set up full disk encryption (LUKS) during install via Calamares
3. Enable WireGuard VPN: `sudo systemctl enable --now wg-quick@wg0`
4. Optionally enable Tor routing system-wide with ProxyChains
5. Run `sudo usbguard generate-policy > /etc/usbguard/rules.conf` after plugging in your trusted devices

---

## Palette

| Token | Hex | Use |
|-------|-----|-----|
| Background | `#0d0d14` | Window backgrounds |
| Surface | `#111118` | Cards, panels |
| Elevated | `#1a1a26` | Buttons, inputs |
| Border | `#2a2a38` | Dividers |
| Accent | `#00ff91` | Primary accent, cursor |
| Accent 2 | `#0087ff` | Secondary accent |
| Muted | `#8888a0` | Subtitles, labels |
| Text | `#ffffff` | Body text |

---

## License

MIT - Voidagon Studios
