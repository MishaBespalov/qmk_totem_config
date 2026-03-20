# TOTEM QMK Keymap — Misha

Ported from OP36 ZMK config. Russian/English dual-layer with home-row mods.

## Layers

### 0: EN — English base

```
        |  F |  D |  L |  B |  V |    |  J |  G |  O |  U |  , |
        |Gs  |At  |Sr  |Cn  |  K |    |  Y |Cm  |Sa  |Ae  |Gi  |
   |   || Z  |  Q |  X |  H |  P |    |  W |  C |  ' |  ; |  . ||   |
                  | SPC| NAV| ESC|    | ENT| SYM| BSP|
```

Home-row mods (hold = modifier, tap = letter): G=GUI, A=ALT, S=SHIFT, C=CTRL.

### 1: RU — Russian (OS must be in Russian mode)

```
        |  й |  ц |  у |  к |  е |    |  н |  г |  ш |  щ |  з |
        |Gф  |Aы  |Sв  |Cа  |  п |    |  р |Cо  |Sл  |Aд  |Gж  |
   |   || я  |  ч |  с |  м |  и |    |  т |  ь |  б |  ю |  э ||   |
                  | SPC| NAV| ESC|    | ENT| SYR| BSP|
```

### 2: SYM_EN — Symbols (English)

```
        |  ~ |  < |  = |  > |  ! |    |  $ |  [ |  _ |  ] |  , |
        |G\  |A(  |S-  |C)  |  + |    |  % |C{  |S?  |A]  |G:  |
   |   || #  |  * |  ` |  / |  & |    |  @ |  | |  " |  ; |  . ||   |
                  |    | NAV|    |    |    |    |    |
```

### 3: SYM_RU — Symbols (Russian OS mode)

Same as SYM_EN except keys that differ in Russian mode use RU-specific keycodes
(e.g. RU comma, RU colon, RU question mark, RU slash, RU double quotes, RU semicolon, RU dot).

### 4: NAV — Navigation / Numbers

```
        |  1 |  2 |  3 |  4 |  5 |    |  6 |  7 |  8 |  9 |  0 |
        | GUI| ALT| SFT| CTL|CS↹ |    |    |C ← |S ↓ |A ↑ |G → |
   |   ||Wh↑ |Wh↓ |RClk|LClk|C↹  |    |    |    |  ^ |    |    ||   |
                  |    |    |    |    |S-ENT| SYE| BSP|
```

## Combos

| Combo | Keys | Layer | Action |
|-------|------|-------|--------|
| х (kha) | G + O (pos 6+7) | RU only | Types х |
| ъ (hard sign) | O + U (pos 7+8) | RU only | Types ъ |
| ё | B + V (pos 3+4) | RU only | Types ё |
| Language toggle | C + ' (pos 27+28) | EN or RU | Switches layer + sends CapsLock |
| Tab | Y + M (pos 15+16) | All | Tab |
| Paste | X + H (pos 23+24) | All | Alt+Ctrl+9 |
| Copy | Q + X (pos 22+23) | All | Ctrl+Alt+8 |
| CapsLock | I + ; + . (pos 19+29+30) | All | CapsLock |
| Vim A | ENT + SYM + BSP (right thumb) | RU only | Types A |
| **Bootloader L** | **F + S + Z (left column)** | **All** | **Enters bootloader (left half)** |
| **Bootloader R** | **,  + I + . (right column)** | **All** | **Enters bootloader (right half)** |

## Flashing

### Prerequisites

QMK firmware is at `~/trackball_firmware/qmk_firmware/`.
Keymap is at `keyboards/totem/keymaps/misha/`.

### Compile

```sh
cd ~/trackball_firmware/qmk_firmware
qmk compile -kb totem -km misha
```

Produces `totem_misha.uf2`.

### Enter bootloader

**Option A — Keyboard combo (recommended):**
- Left half: press **F + S + Z** simultaneously (3 leftmost column keys)
- Right half: press **,  + I + .** simultaneously (3 rightmost column keys)
- The half connected via USB will reboot into bootloader

**Option B — Hardware button:**
1. Unplug USB
2. Hold the **BOOT** button on the XIAO RP2040 controller
3. Plug USB in while holding BOOT
4. Release BOOT

A drive called `RPI-RP2` should appear.

### Flash

```sh
# Find the drive
lsblk -o NAME,LABEL | grep RPI-RP2

# Mount and copy (replace sdX1 with actual device)
sudo mount /dev/sdX1 /mnt
sudo cp ~/trackball_firmware/qmk_firmware/totem_misha.uf2 /mnt/
sudo sync
```

The keyboard auto-reboots after copying.

**Both halves need the same firmware.** Flash the left half first, then the right half.

### Normal operation

Plug the **left half** into USB (it is the master). Connect the right half via the TRRS/serial cable.

## OS requirements

- Keyboard layouts: **US + Russian** (`us,ru`)
- Language toggle: **CapsLock** (`grp:caps_toggle`)

Example Hyprland/sway config:
```
input {
    kb_layout = us,ru
    kb_options = grp:caps_toggle
}
```

## Files

| File | Purpose |
|------|---------|
| `keymap.c` | Layers, combos, macros, custom keycodes |
| `config.h` | Timing (250ms tapping term, 175ms quick-tap, permissive hold) |
| `rules.mk` | Enables combo and mousekey features |
