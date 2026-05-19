# QMK Keyboard Converter

A QMK keyboard converter allows for using custom keyboard keybindings with any
keyboard. Plug your keyboard into the converter and it will translate
the keypresses into custom keycodes according to your keymap.

## Why?

If you have a retro / standard office keyboard, it probably doesn't have any keymap
remapping software. Of course, you could use some software layer running on
your OS (e.g. AutoHotKey) to remap the keys. One limitation is that it is not
portable across different machines. With this converter, you can connect it to
any PC / keyboard combination and it will have your keybindings.

I'm using this converter setup for [Unicomp Mini M](https://www.pckeyboard.com/page/product/MINI_M)
and [Logitech Deluxe 250](https://docs.rs-online.com/fad5/0900766b80df83ee.pdf)
and it works great so far.

## Quick Start

### Prerequisites

1. You need and Arduino Leonardo with USB Host Shield (refer to [hardware](#hardware) section)
2. You need `avrdude` installed on your machine.

### Minimal Steps

1. Define and compile your keymap in the [QMK Configurator](https://config.qmk.fm/#/converter/usb_usb/leonardo/LAYOUT_all)
   - Define the keycode overrides & layers using the web editor
   - Compile your keymap
   - Download firmware for flashing
4. Flash using the `flash` script provided in this repository.

## Advanced Setup

> Note: this is only useful if you want to e.g. send mouse movement events with
  your keyboard or override some advanced capabilities.

First, you need to clone the `qmk_firmware` repository that allows to compile the
keybindings. To do this, you need to:

```
git submodule update --init --recursive
```

That will clone the firmware repository.

### Defining the keymap

To define the desired keymap, you will need to use the QMK web UI.
You need to pick the [converter/usb_usb/leonardo](https://config.qmk.fm/#/converter/usb_usb/leonardo/LAYOUT_all)
keyboard type.

Once you define the keymap, save it down as json so that it can be
compiled locally.

### Customizing the converter capabilities

You can customize the `usb_usb` converter capabilities by modifying the files:
```
keyboards/converter/usb_usb/config.h
keyboards/converter/usb_usb/rules.mk
```

For example, I enabled mouse support and adjusted the mouse pointer speed
for my configuration with the following diff:

```
--- a/keyboards/converter/usb_usb/config.h
+++ b/keyboards/converter/usb_usb/config.h
@@ -21,7 +21,8 @@ along with this program.  If not, see <http://www.gnu.org/licenses/>.
 /* size of virtual matrix */
 #define MATRIX_ROWS 16
 #define MATRIX_COLS 16
-
+#define MOUSEKEY_MOVE_DELTA 6
+#define MOUSEKEY_MAX_SPEED 6
 /*
  * Feature disable options
  *  These options are also useful to firmware size reduction.
--- a/keyboards/converter/usb_usb/rules.mk
+++ b/keyboards/converter/usb_usb/rules.mk
@@ -2,7 +2,7 @@
 #   change yes to no to disable
 #
 BOOTMAGIC_ENABLE = no       # Enable Bootmagic Lite
-MOUSEKEY_ENABLE = no        # Mouse keys
+MOUSEKEY_ENABLE = yes        # Mouse keys
 EXTRAKEY_ENABLE = yes       # Audio control and System control
 CONSOLE_ENABLE = no         # Console for debug
 COMMAND_ENABLE = no         # Commands for debug and configuration
```

### Compiling the firmware

Navigate to the `qmk_firmware` folder and execute
```
qmk compile <name of your json keymap file>
```

This should create the `.hex` file that you can then take and flash onto your target
microcontroller.
