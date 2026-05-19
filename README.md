# QMK Keyboard Converter

QMK keyboard converter allows for using custom keyboard keybindings with any
keyboard. Simply plug the keyboard into the converter and it will translate
the actual physical keypresses on your keyboard into custom keycodes sent
by the converter.

## Why?

If you have a retro / simple keyboard, it probably doesn't have any keymap
remapping software. Of course, you could use some software layer running on
your OS (e.g. AutoHotKey) to remap the keys. The problem is that this is not
portable across different machines.

With the USB converter you can use you custom QMK keybindings on any keyboard
and machine.

In my case, I need this converter for my [Unicomp Mini M](https://www.pckeyboard.com/page/product/MINI_M)

## Quick Start

### Prerequisites

1. You need and Arduino Leonardo with USB Host Shield (refer to [hardware](#hardware) section)
2. You need `avrdude` installed on your machine.

### Minimal Setup: 0 to flashing sample firmware

1. Define your custom keymap in the [QMK Configurator](https://config.qmk.fm/#/converter/usb_usb/leonardo/LAYOUT_all)
2. Compile your keymap (can be done directly in the configurator web UI)
3. Download firmware for flashing
4. Flash using the `flash` script provided in this repository.

## Getting started

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

Once you define the keymap, you can save it down as json so that it can be
restored in the web UI in the future (in case want to tweak it further).

You can also use the QMK UI to compile the keymap and get the `.hex` file that
can be directly flashed onto the Arduino.


## Customizing the converter capabilities

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
