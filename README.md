# zmk-config-totem

ZMK firmware config for a **TOTEM** 38-key split, on **Seeed XIAO BLE (nRF52840)**.
Board target: `xiao_ble/nrf52840/zmk` (ZMK main / Zephyr 4.1, hardware model v2).

## This config supports ZMK Studio (live config, no reflash)

The TOTEM shield is **vendored locally** under `boards/shields/totem/` (copied from
[`rcarmo/zmk-config-totem`](https://github.com/rcarmo/zmk-config-totem)) because it ships the
`physical-layouts` + `totem.zmk.yml` metadata that ZMK Studio requires. `zephyr/module.yml`
(`board_root: .`) makes the local shield discoverable. **Do not delete these** — without them
Studio cannot run and you are back to hand-editing + reflashing.

Studio is enabled for the **left/central half only**, via `build.yaml`:
`snippet: studio-rpc-usb-uart` + `cmake-args: -DCONFIG_ZMK_STUDIO=y`.

### To remap keys the easy way
1. Plug the **left** half into USB.
2. Open ZMK Studio (https://zmk.studio or the desktop app) → connect over USB.
3. On the keyboard, reach the **ADJ** layer (hold both inner thumbs: SPACE + BACKSPACE) and tap
   **studio_unlock** (left-hand home row, middle key) to allow editing.
4. Remap live. No rebuild, no flash.

## Layout notes
- Home-row mods use custom `hml`/`hmr` hold-taps tuned for fast typing:
  `require-prior-idle-ms = 150` (just-typed keys never become mods) +
  positional `hold-trigger-key-positions` (only opposite-hand presses trigger a mod).
- Layers: 0 base · 1 num · 2 media · 3 nav · 4 adjust (conditional: nav+num).
- **Left half is the central side** — USB power/data goes there; it relays the right half over BLE.

## Building / flashing (only needed for firmware-level changes, not key remaps)
1. `git push` → GitHub Actions builds, artifacts in the Actions tab.
2. Per half: USB in, double-tap reset → drag the matching `.uf2` onto the `XIAO-BLE` drive.
3. If Bluetooth/pairing misbehaves: flash `settings_reset` to **both** halves first, then the
   normal firmware, then re-pair.
