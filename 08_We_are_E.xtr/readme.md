# We are E.xtr

* Category: Forensics

## Description

[E.xtr](E.extr)

## Solution

Let's try to find some more info about the file:

```bash
$ file E.xtr 
E.xtr: data
```

Uhm, not so helpful... let's go deeper inside the thing:

```bash
$ xxd E.xtr 
00000000: 8958 5452 0d0a 1a0a 0000 000d 4948 4452  .XTR........IHDR
00000010: 0000 0500 0000 02d0 0803 0000 018f a41d  ................
00000020: f200 0000 0173 5247 4200 aece 1ce9 0000  .....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0066 504c 5445 ffff ffdf dfdf 7f7f 7f40  .fPLTE.........@
[...]

```

If you know something about graphics stuff, some of those strings (e.g: sRGB) might ring a bell.
Let's search the Internet: it appears that what we have here is a PNG file: http://www.libpng.org/pub/png/spec/1.2/PNG-Chunks.html
Unfortunately we are not able to open it as-is as the signature (header) is not right (should spell PNG instead of XTR).
Let's just fix it with an hex editor and we get the flag:

![E.png](E.png)
