# Breakfast

* Category: Cryptography

## Description

```
Mmm I ate some nice bacon and eggs this morning. Find out what else I had for
an easy flag. Don’t forget to capitalize CTF! [Ciphertext](enc.txt)
```

## Solution

The code is:

> 011100010000000000101001000101{00100001100011010100000000010100101010100010010001}


As the description suggests, we are dealing with a [Baconian (or Bacon's) Cipher](https://en.wikipedia.org/wiki/Bacon%27s_cipher)

This is somehow similar to ASCII encoding. While ASCII considers 128 different symbols (7 bit), here we are dealing with just 26 letters, so 5 "bits" are enough to cover all of them.
Here's a table with the conversion:

| Letter | Binary | Letter | Binary |
| ------ | ------ | ------ | ------ |
| A      | 00000  | N      | 01101  |
| B      | 00001  | O      | 01110  |
| C      | 00010  | P      | 01111  |
| D      | 00011  | Q      | 10000  |
| E      | 00100  | R      | 10001  |
| F      | 00101  | S      | 10010  |
| G      | 00110  | T      | 10011  |
| H      | 00111  | U      | 10100  |
| I      | 01000  | V      | 10101  |
| J      | 01001  | W      | 10110  |
| K      | 01010  | X      | 10111  |
| L      | 01011  | Y      | 11000  |
| M      | 01100  | Z      | 11001  |


Let's then split the original cipher in groups of 5 bits:

> 01110 00100 00000 00010 10010 00101 { 00100 00110 00110 10100 00000 00101 00101 01010 00100 10001 }

Looking up in the table we can easily tell that the decoded text is:

PEACTF{EGGWAFFLES}

We could also translate the code to standard ASCII by adding 1000001b to each Bacon symbol.
