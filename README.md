# QMK Keyboard Converter

QMK keyboard converter allows for using custom keyboard keybindings with any
keyboard. Simply plug the keyboard into the converter and it will translate
the actual physical keypresses on your keyboard into the custom keycodes sent
by the converter.

## Why?

If you have a retro / simple keyboard, it probably doesn't have any keymap
remapping software. Of course, you could use some software layer running on
your OS (e.g. AutoHotKey) to remap the keys. The problem is that this is not
portable across different machines.

With the USB converter you can use you custom QMK keybindings on any keyboard
and machine.

In my case, I need this converter for my (Unicomp Mini M)[https://www.pckeyboard.com/page/product/MINI_M]

## Getting started

First, you need to clone the `qmk_firmware` repository that allows to compile the
keybindings.

