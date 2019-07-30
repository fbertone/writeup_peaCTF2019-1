# Broken Keyboard

* Category: Cryptography

## Description

Help! My keyboard only types numbers! [Ciphertext](enc.txt)

## Solution

The "encoded" text is:

> 112 101 97 67 84 70 123 52 115 99 49 49 105 115 99 48 48 108 125

We can easily guess that's just ASCII code in decimal base.

We can use an online [tool](https://www.rapidtables.com/convert/number/ascii-hex-bin-dec-converter.html) for a quick conversion.

This translates to:

> cGVhQ1RGezRzYzExaXNjMDBsfQ==

The two trailing '==' always smell like base64 encoding.

We can use another online [tool](https://www.base64decode.org/) to complete the challenge and get the flag.

**peaCTF{4sc11isc00l}**
