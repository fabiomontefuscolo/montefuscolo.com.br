---
title: "Missing slash Key in Archlinux Install"
date: 2013-05-11T23:55:00-03:00
draft: true
summary: "Archlinux misses the mapping of `/` key for some keyboard brands, which makes the installation process a bit hard"
tags: ["linux"]
---

Samsung Chronos 7 keyboard has the `/` key as secondary for the key `Q`. To use the key `/` in the Archlinux installation it is necessary to add a mapping like `<Altgr> + q`.

### 1. Detects a keycode from the key pressed on the keyboard.
```shell
root@archiso ~ # showkey
kb mode was UNICODE
[ if you are trying this under X, it might not work
since the X server is also reading /dev/console ]

press any key (program terminates 10s after last keypress)...
keycode  28 release
keycode  16 press
keycode  16 release
```

### 2. Decompress the map file
```shell
root@archiso ~ # gunzip /usr/share/kbd/keymaps/i386/qwerty/br-abnt2.map.gz
```

### 3. Edit the file using any text editor (here is Vim)
```shell
root@archiso ~ # vi /usr/share/kbd/keymaps/i386/qwerty/br-abnt2.map
```

### 4. Add the following to the file
```
altgr   keycode  16 = slash
```

### 5. Compress the file again
```shell
root@archiso ~ # gzip /usr/share/kbd/keymaps/i386/qwerty/br-abnt2.map
```

### 6. Load the new map file
```shell
root@archiso ~ # loadkeys br-abnt2
```