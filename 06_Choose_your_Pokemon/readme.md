# Choose your Pokemon

* Category: Forensics

## Description

Just a simple type of recursive function. [master-ball](master-ball)

## Solution

```bash
$ file master-ball 
master-ball: RAR archive data, v5
```

```bash
$ unrar x master-ball

UNRAR 5.61 beta 1 freeware      Copyright (c) 1993-2018 Alexander Roshal


Extracting from master-ball

Extracting  roshambo                                                  OK 
All OK
```


```bash
file roshambo
roshambo: Zip archive data, at least v2.0 to extract
```

```bash
$ unzip roshambo
Archive:  roshambo
  inflating: inDesign
```

```bash
file inDesign 
inDesign: PDF document, version 1.7
```

Let's open the file with a pdf reader:

> https://pastebin.com/AWTDEb9j

A bunch of text shows up
Let's paste all of it in a file "pastebin"

```bash
file pastebin 
pastebin: Rich Text Format data, version 1, unknown character set
```

Let's open it in a text editor (pick one that support RTF format or it will not work)

> {wild_type}
