# FCSC24 Writeup ~ AdveRSArial Crypto (Infant)

## Challenge Description

- **Title:** AdveRSArial Crypto (Infant)
- **Category:** Intro
- **Points:** 20

```
Je viens de suivre un cours sur RSA mais je crois que j'ai oublié quelque chose. Il me semble que le prof parlait de deux trucs, mais je ne sais plus exactement quoi.
```

## Understanding the Challenge

This challenge gives us 2 files:
```
adversarial-crypto-infant.py: Python script, ASCII text executable
output.txt:                   ASCII text, with very long lines (621)
```

`output.txt` seems to contain an `RSA private key` and encrypted message


## Solution

I open up `adversarial-crypto-infant.py` with subl

I instantly see `n` is not set to be the product of 2 primes, but directly to a large number

I create my own python file to decrypt the message

Since `n` is not the product of primes theres no `p, q` for generating `d`, the private key, therefore I decrypt using `p, q = n`

Running the script:

```
from Crypto.Util.number import getStrongPrime, bytes_to_long, long_to_bytes

d = pow(e, -1, (n-1)*(n-1))
flag = pow(c, d, n)
flag = long_to_bytes(flag)

print(flag)
```

We get the flag:
```FCSC{d0bf88291bcd488f28a809c9ae79d53da9caefc85b3790f57615e61c70a45f3c}```