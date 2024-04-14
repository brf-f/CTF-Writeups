# FCSC24 Writeup ~ Fifty Shades of White (Junior)

## Challenge Description

- **Title:**
- **Category:** intro, reverse
- **Points:** 20

```
Le grand Walter White a écrit un programme lui permettant de restreindre l'accès à ses données "professionnelles". Il distribue des licenses au compte-gouttes, mais vous avez néanmoins récupéré une license qu'il a générée pour son fils ! Son système propose deux niveaux de licenses : celle que vous avez récupérée est la moins privilégiée, et vous souhaitez obtenir une license "admin".

Le programme ci-joint vérifie entre autres le niveau de privilèges de la license, et vous récompense si vous présentez une license "admin".
```

## Understanding the Challenge

This challenge gives us 2 files:

```
bin_ex.elf:  ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=b2d9be8969f6d06075b62cc8911f74c1eab7dd2d, for GNU/Linux 3.2.0, not stripped

license.txt: ASCII text
```

and a `nc` instance: `nc challenges.france-cybersecurity-challenge.fr 2250`

Running the netcat instance asks for a license, I assume we get the flag when providing correct license

Looking at the provided license file we can see its encoded in base64, decoding gives:
```
----BEGIN WHITE LICENSE----
Name: Walter White Junior
Serial: 1d117c5a-297d-4ce6-9186-d4b84fb7f230
Type: 1
-----END WHITE LICENSE-----
```

We can guess that the other license will follow a similar format

## Solution

I open the program on `ghidra` and start renaming variables, and understanding the structure of the program

the `parse` function checks that the `license.txt` matches the correct format

and sets `(m_license + 2)` equal to the value of `Type` from the license

A `validate` function is then run with the `serial`

`check` then returns the flag if `(m_license c + 2) == 1337`

We simply set `Type = 1337`, encode it into Base64 while maintaining the previous format giving:

```
----BEGIN WHITE LICENSE----
TmFtZTogV2FsdGVyIFdoaXRlIEp1bmlvcgpTZXJpYWw6IDFkMTE3YzVhLTI5N2QtNGNlNi05MTg2LWQ0Yjg0ZmI3ZjIzMApUeXBlOiAxMzM3Cg==
-----END WHITE LICENSE-----

```

 and get the flag:

```FCSC{2053bb69dff8cf975c1a3e3b803b05e5cc68933923aabdd6179eace1ece0c41a}```