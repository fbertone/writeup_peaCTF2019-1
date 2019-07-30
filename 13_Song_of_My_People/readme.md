# Song of My People

* Category: Forensics

## Description

A specific soundcloud rapper needs help getting into his password protected zipped file directory. The initial password is in the title. You just have to know your memes, and pick the right instrument! We were on the fence on giving you an image to go along with this puzzle, but the loincloth was too scandalous. Alternatively, you could bruteforce. [Song of My People](song_of_my_people.zip)

## Solution

This was the crazies challenge.
I don't get the meme reference so I went for the bruteforce option...


## Part 1: cracking the zip file

```bash
zip2john song_of_my_people.zip > song.key
ver 2.0 efh 9901 song_of_my_people.zip/Ice Cube - Check Yo Self Remix (Clean).mp3 PKZIP Encr: cmplen=5550839, decmplen=5601208, crc=3F7D5D
ver 2.0 efh 9901 song_of_my_people.zip/README.txt PKZIP Encr: cmplen=132, decmplen=123, crc=E3A5855B
ver 2.0 efh 9901 song_of_my_people.zip/a lengthy issue.png PKZIP Encr: cmplen=42909, decmplen=44525, crc=6514CE68
NOTE: It is assumed that all files in each archive have the same password.
If that is not the case, the hash may be uncrackable. To avoid this, use
option -o to pick a file at a time.
```

```bash
john song.key
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 256/256 AVX2 8x])
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Warning: Only 7 candidates buffered for the current salt, minimum 8 needed for performance.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
violin           (song_of_my_people.zip/Ice Cube - Check Yo Self Remix (Clean).mp3)
1g 0:00:00:04 DONE 2/3 (2019-07-30 11:05) 0.2331g/s 18222p/s 18222c/s 18222C/s tricky..121212
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```bash
john song.key --show
song_of_my_people.zip/Ice Cube - Check Yo Self Remix (Clean).mp3:violin:Ice Cube - Check Yo Self Remix (Clean).mp3:song_of_my_people.zip:song_of_my_people.zip

1 password hash cracked, 0 left
```

The password for the zip file is: *violin*

Note: unzip does not support AES encrypted files, so you would get this error:

```bash
$ unzip song_of_my_people.zip 
Archive:  song_of_my_people.zip
   skipping: Ice Cube - Check Yo Self Remix (Clean).mp3  unsupported compression method 99
   skipping: README.txt              unsupported compression method 99
   skipping: a lengthy issue.png     unsupported compression method 99
```

Fortunately 7zip is ok with that:

```bash
$ 7z x -pviolin song_of_my_people.zip 

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,1 CPU Intel(R) Core(TM) i5-4690K CPU @ 3.50GHz (306C3),ASM,AES-NI)

Scanning the drive for archives:
1 file, 5594494 bytes (5464 KiB)

Extracting archive: song_of_my_people.zip
--
Path = song_of_my_people.zip
Type = zip
Physical Size = 5594494

Everything is Ok

Files: 3
Size:       5645856
Compressed: 5594494
```

```bash
$ cat README.txt 
one of the three files is a red herring, but a helpful one at that.

does any of this ADD up? This is a LONG problem.
```
The mp3 file seems to be a regular mp3 file, while the png file seems to be broken as it cannot be opened in graphics applications.


## Part 2: Recovering the PNG file

It's time to kick ass and chew bubble gum... and I'm all outta gum.
No, wait... I mean... it's time to open the [PNG format specification](https://www.w3.org/TR/2003/REC-PNG-20031110/#5DataRep): 

A PNG file consists of a PNG *signature* followed by a series of *chunks*.

The first eight bytes of a PNG datastream always contain the following (decimal) values:

```
   137 80 78 71 13 10 26 10
```

This signature indicates that the remainder of the datastream contains a single PNG image, consisting of a series of chunks beginning with an IHDR chunk and ending with an IEND chunk.

A chunk consists of four parts: length (4 bytes, big-endian), chunk type/name (4 bytes), chunk data (length bytes) and CRC (cyclic redundancy code/checksum; 4 bytes). The CRC is a network-byte-order CRC-32 computed over the chunk type and chunk data, but not the length.

| Length  | Chunk type | Chunk data     | CRC     |
| ------- | ---------- | -------------- | ------- |
| 4 bytes | 4 bytes    | *Length* bytes | 4 bytes |

For the moment that's enough information. Let's check the file.

The first eight bytes are the regular signature: 89 50 4E 47 0D 0A 1A 0A. No issues here.

What follow is then the first chunk's header:

| Length      | Chunk type | Chunk data | CRC         |
| ----------- | ---------- | ---------- | ----------- |
| 00 00 00 0D | IHDR       | [10] bytes | 8F A4 1D F2 |

The first chunk seems to be OK.

Let's go to the second:

| Length      | Chunk type | Chunk data | CRC         |
| ----------- | ---------- | ---------- | ----------- |
| 00 00 00 01 | sRGB       | 00         | AE CE 1C E9 |

This also seems fine. Go on...



| Length      | Chunk type | Chunk data  | CRC         |
| ----------- | ---------- | ----------- | ----------- |
| 00 00 00 04 | gAMA       | 00 00 B1 8F | 0B FC 61 05 |

Same thing, everything seems OK. Next...



| Length             | Chunk type | Chunk data | CRC  |
| ------------------ | ---------- | ---------- | ---- |
| 48 45 4C 50 (HELP) | PLTE       | ...        | ...  |

A chunk of size "HELP"? That's interesting...

For sure the size of this chunk cannot be ‭1.212.501.072‬ bytes (48454C50h in decimal) since the total file size is just 44.525 bytes.

Let's try to find another following chunk so we can guess the real size.

After some bytes we can see the header for a tRNS chunk (starting at offset 20Fh).

Since the PLTE chunk data starts at offset 46h, and considering the 4 trailing bytes for the checksum, the total size for our chunk is 1C5h (453) bytes.

Lets' fix the length field with this bytes: 00 00 01 C5.

Voila, now we can see the original file.

OK, a lot of things in it. Some instructions, a link and some encoded data at the bottom.

> 54 68 65 20 4C 69 62 72 61 72 79 20 6F 66 20 42 61 62 65 6C 3A 0A 28 77 69 74 68 20 6E 65 77 20 61 64 64 69 74 69 6F 6E 20 6F 66 20 61 6C 20 74 68 65 20 70 6F 73 69 62 6C 65 20 64 69 73 73 20 74 72 61 63 6B 73 20 74 6F 20 65 76 65 72 20 62 65 20 6D 61 64 65 20 61 6E 64 20 65 76 65 72 20 63 6F 75 6C 64 20 62 65 20 6D 61 64 65 29

The numbers at the bottom are obviously ASCII codes, that translates to:

> The Library of Babel:
> (with new addition of all the possible diss tracks to ever be made and ever could be made)

Nothing really interesting shows up after a search on google.



## Part 3: SoundCloud decoding



Coming back to the link: at the beginning I was thinking I was supposed to find the [redacted] part of the link, so I started searching for lil-something rappers on soundcloud.

After a while I discovered that there *is* a lil-redacted account, so it was actually the real link to follow.

The description for the track is:

> this concert is part of a larger tour that is archived  completely in some kind of hexagonal library. The archive is named  between "maybe" and a "repeat". Should be on the 371st page.
>
> I would give you an mp3 of this audio, but I don't know how to navigate those sketchy websites.

This is somehow related to the text in the image but doesn't really make sense to me at this point.

Let's listen to the audio. A series of impulse tones... feels like Morse code to me.

I downloaded the audio using something like https://sclouddownloader.com/

I used https://morsecode.scphillips.com/labs/audio-decoder-adaptive/ (very cool tool!) to decode the

mp3 audio and got this text:

> SUP YALL ITS YA BOI LIL ICE CUBE MELTING OUT HERE IN THE HAWAII HEAT FOR ALL OF YOU. YOU GUESSED IT THIS IS LIVE AUDIO FROM MY WORLD TOUR. I REPEAT LIL ICE CUBES WORLD TOUR MAYBE A LIBRARY WILL HELP

Well, another reference to a library, at this point I can not ignore it...



# Part 4: The Library...



Recap: we have a reference to the "Library of Babel" and a "hexagonal library", plus "sketchy websites".

Let's ask Google...

Turns out there is this https://libraryofbabel.info/ website that has hexagons as logo... sounds familiar.

I don't really know what it is about, seems like some research project, however there's a search function that could be useful...

Searching for "lil ice cubes world tour" results in:

> exact match:
>
> Title: weif.rfgkd,zvjxxd Page: 371
> Location: 2nhptm68h2sputfsdlbmi75s6qcfgu...-w2-s4-v08

Page 371... we are in the right direction!

Click on the link and open the page... after the title we can see a lot of *free space(s)*!

We can copy the page and count the spaces, however we already know that each page in the website contains 3200 characters, which means 3 thousands spaces.

This gives us all the information to construct the flag: 

**{3_thousand_spaces_371}**

