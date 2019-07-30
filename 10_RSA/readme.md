# RSA

* Category: Cryptography

## Description

Can you help Bob retrieve the two messages for a flag? Authenticated Channel Encrypted Channel

## Solution

As the title suggests, we are dealing with RSA algorithms.
I'll use the tool RsaCtfTool.

We have 2 files: the first refers to *enc*ryption, the second to *auth*entication.

Inside the files we can see the numeric parameters corresponding to the keys, however we need to crack the missing parameters in order to find the keys.

Encrypted channel:
```bash
$ ./RsaCtfTool.py -n 165481207658568424313022356820498512502867488746572300093793 -e 65537 --uncipher 150635433712900935381157860417761227624682377134647578768653
[+] Clear text : b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00peaCTF{f4ct0r'
```
We can already see the first half of the flag: peaCTF{f4ct0r

Authenticated channel:
```bash
$ ./RsaCtfTool.py -n 59883006898206291499785811163190956754007806709157091648869 -e 65537 --uncipher 23731413167627600089782741107678182917228038671345300608183
[+] Clear text : b'\x06s\x08\x04\xb4\xb8FX\xcbU\x1a\x0f\x84\xe4\xdb\xb8\xe0\x99\x1e\xd9\x90=9\xf1g'
```
In this case we are not as lucky as before: the decoded data makes no sense.
But let's think a bit more about the inputs: one is encrypted data, the other is authenticated.
This means that the first one is encrypted with the public key and can be decrypted with the private key,
while for the second it should be the other way around.
(If you don't understand why read something about Public Key cryptography and how it can be used to sign/authenticate data)

Let's the try to get all the keys in order to have a complete view.
