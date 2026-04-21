# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK firmware configuration repository for the Lily58 split keyboard, targeting the nice!nano v2 microcontroller. Firmware is built via GitHub Actions — there is no local build toolchain in this repo.

## Building Firmware

**CI build (primary method):** Push to `main` or trigger via GitHub Actions (`workflow_dispatch`). The workflow at `.github/workflows/build.yml` calls the ZMK reusable workflow and produces left/right `.uf2` firmware artifacts.

**Local build (optional):** Requires a Zephyr/west development environment. The `.zmk/` directory (gitignored) holds the ZMK workspace initialized with `west init` and `west update` using `config/west.yml` as the manifest.

## Repository Structure

- `config/lily58.keymap` — keymap definition in ZMK's devicetree binding format (3 layers: default, lower, raise)
- `config/lily58.conf` — Kconfig options for enabling features (encoder, OLED display)
- `config/west.yml` — west manifest pinning ZMK to `v0.3`
- `build.yaml` — defines the GitHub Actions build matrix (nice_nano_v2 + lily58_left/lily58_right)
- `boards/shields/` — placeholder for custom board/shield definitions
- `zephyr/module.yml` — identifies this repo as a Zephyr module

## Keymap Architecture

The keymap uses ZMK's devicetree syntax. Key concepts:
- `&kp KEY` — key press behavior
- `&mo N` — momentary layer activation (thumb keys activate lower/raise)
- `&bt BT_*` — Bluetooth profile management (on lower layer)
- `&ext_power EP_*` — external power control (on lower layer)
- `sensor-bindings` — rotary encoder mapped to volume up/down on all layers

Layers: **0** = default QWERTY, **1** = lower (BT controls + F-keys + symbols), **2** = raise (numrow + arrows + F-keys)

## Flashing

After downloading firmware artifacts from GitHub Actions:
1. Double-tap reset on the nice!nano to enter bootloader (appears as USB drive)
2. Copy `lily58_left-nice_nano_v2-zmk.uf2` to left half, `lily58_right-nice_nano_v2-zmk.uf2` to right half
