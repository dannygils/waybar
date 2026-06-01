# AGENTS.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Overview

This is a [Waybar](https://github.com/Alexays/Waybar) configuration for a Hyprland (Wayland) desktop on NixOS. Waybar is a status bar; this repo defines its layout, module behavior, and visual theme.

## Files

- `config.jsonc` — Module layout and behavior (JSONC format: JSON with `//` comments)
- `style.css` — Visual styling using GTK3 CSS (not standard web CSS—some properties differ)

## Applying Changes

Waybar reads config on startup from `~/.config/waybar/`. To reload after editing:

```sh
killall waybar && waybar &
```

Or send SIGUSR2 to reload config without restarting:

```sh
pkill -SIGUSR2 waybar
```

## Architecture

### config.jsonc

The bar is split into three sections defined by `modules-left`, `modules-center`, and `modules-right`. Each entry references a module by name.

- **Built-in modules** use a namespace prefix (e.g. `hyprland/workspaces`, `clock`, `pulseaudio`, `network`, `tray`). Their options are documented in the [Waybar wiki](https://github.com/Alexays/Waybar/wiki).
- **Custom modules** use the `custom/` prefix (e.g. `custom/firefox`, `custom/power`). These are app launchers or utility buttons configured inline with `format`, `on-click`, and `tooltip` fields.

Key conventions in this config:
- Icons use Nerd Fonts glyphs (requires a Nerd Font such as DejaVuSansMono Nerd Font).
- `on-click` / `on-click-right` execute shell commands directly.
- `interval: 0` means the module is static (no polling).

### style.css

Modules are targeted by GTK CSS selectors using `#module-name` (e.g. `#clock`, `#custom-firefox`). The theme uses a dark palette with rounded pill-shaped modules (`border-radius: 999px`) and semi-transparent backgrounds.

When adding a new custom module, you must add both:
1. The module definition in `config.jsonc`
2. Its selector to the shared styling rule in `style.css` (the comma-separated `#pulseaudio, #network, ...` block) and optionally an accent color rule.

## Conventions

- Use JSONC (not plain JSON) so comments remain valid.
- Icon glyphs come from [Nerd Fonts cheat sheet](https://www.nerdfonts.com/cheat-sheet).
- The color palette follows a Nord/Tokyo Night aesthetic—match existing colors when adding modules.
