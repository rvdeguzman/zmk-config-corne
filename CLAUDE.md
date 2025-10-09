# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK firmware user configuration repository for a Corne keyboard with nice!view displays. ZMK is an open-source keyboard firmware built on the Zephyr RTOS. This repository contains keymap configurations that are automatically compiled into firmware via GitHub Actions.

## Architecture

### Configuration Structure

- **config/corne.keymap**: Main keymap definition using devicetree syntax. Defines three layers:
  - Layer 0 (Default): Standard QWERTY layout with mod-taps
  - Layer 1 (Lower): Numbers, Bluetooth controls, and navigation arrows
  - Layer 2 (Raise): Symbols and special characters

- **config/corne.conf**: Hardware feature toggles (RGB underglow, OLED display)

- **config/west.yml**: West manifest specifying ZMK version (v0.3) and dependencies

- **build.yaml**: GitHub Actions matrix configuration. Currently builds for:
  - `nice_nano_v2` board with `corne_left`, `nice_view_adapter`, and `nice_view` shields
  - `nice_nano_v2` board with `corne_right`, `nice_view_adapter`, and `nice_view` shields

### Build System

Builds are handled entirely by GitHub Actions using the upstream ZMK build workflow. The `.github/workflows/build.yml` delegates to `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3`.

## Common Commands

**Note**: This repository has no local build commands. All firmware compilation happens in GitHub Actions.

### Triggering a Build

Builds automatically trigger on:
- Push to any branch
- Pull request creation
- Manual workflow dispatch via GitHub UI

### Modifying the Keymap

Edit `config/corne.keymap` directly. The file uses devicetree syntax:
- Key positions are defined in the `bindings` array
- Use `&kp` for key presses, `&mo` for momentary layer activation, `&bt` for Bluetooth controls
- Layer activation is zero-indexed (layer 1 = `&mo 1`)

### Enabling Hardware Features

Edit `config/corne.conf` and uncomment desired features:
- `CONFIG_ZMK_RGB_UNDERGLOW=y` and `CONFIG_WS2812_STRIP=y` for RGB
- `CONFIG_ZMK_DISPLAY=y` for OLED/nice!view display

### Changing Build Targets

Edit `build.yaml` to modify board/shield combinations. See comments in the file for syntax examples, including how to add ZMK Studio support or build for different keyboards.

## Keymap Devicetree Syntax

ZMK keymaps use devicetree format:
- Behaviors are prefixed with `&` (e.g., `&kp`, `&mo`, `&bt`)
- Key codes are from `dt-bindings/zmk/keys.h` (e.g., `TAB`, `LCTRL`, `N1`)
- Bluetooth codes are from `dt-bindings/zmk/bt.h` (e.g., `BT_CLR`, `BT_SEL 0`)
- `&trans` preserves the key from the layer below
- Comments use C-style `//` and `/* */`

## West Manifest

The `config/west.yml` file specifies the ZMK version and remote source. Changing the `revision` field updates the ZMK version used for builds (e.g., `v0.3`, `main`).
