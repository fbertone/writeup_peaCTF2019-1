# School

* Category: Cryptography

## Description

My regular teacher was out sick so we had a *substitute* today. [Ciphertext](enc.txt)

## Solution

```
Alphabet: WCGPSUHRAQYKFDLZOJNXMVEBTI
zswGXU{ljwdhsqmags}
```

This is a simple substitution cipher and we are given the equivalent alphabet (W = A, C = B, G = C,...) , so it's just a matter of substitute each character with the right one:

```
W C G P S U H R A Q Y K F D L Z O J N X M V E B T I
| | | | | | | | | | | | | | | | | | | | | | | | | |
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

```
zswGXU{ljwdhsqmags}
|||||||||||||||||||
peaCTF{orangejuice}
```

