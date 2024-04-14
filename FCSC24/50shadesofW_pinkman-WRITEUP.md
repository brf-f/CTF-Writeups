# FCSC24 Writeup ~ Fifty Shades of White (Pinkman)

## Challenge Description

- **Title:** Fifty Shades of White (Pinkman)
- **Category:** Reverse
- **Difficulty:** 1/3

```
Vous incarnez désormais Jesse Pinkman. Vous êtes actuellement en froid avec Walter White et souhaitez lui faire un sale coup.

On vous demande d'écrire un keygen pour le binaire que Walter a conçu pour restreindre l'accès à ses données "professionnelles". Le flag apparaîtra après 50 validations successives.
```

## Understanding the Challenge

This challenge is following the 'Junior' one in the series

We are given 2 files:

```
fifty-shades-of-white:           ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=b2d9be8969f6d06075b62cc8911f74c1eab7dd2d, for GNU/Linux 3.2.0, not stripped

license-walter-white-junior.txt: ASCII text
```
`license.txt` is the same as before, same with the binary file

We are also given a `nc` instance at `nc challenges.france-cybersecurity-challenge.fr 2250`

Previously we found the admin license for Junior to be:

```
----BEGIN WHITE LICENSE----
TmFtZTogV2FsdGVyIFdoaXRlIEp1bmlvcgpTZXJpYWw6IDFkMTE3YzVhLTI5N2QtNGNlNi05MTg2LWQ0Yjg0ZmI3ZjIzMApUeXBlOiAxMzM3Cg==
-----END WHITE LICENSE-----

```

We need to create a keygen to find the licenses for the other users too

## Solution

I start by opening the same project as last time, since we already renamed variables

I already know we want to specify `Type: 1337` for all, now we just need to check what the `Name:` and `Serial:` should be

We know the name will be what they ask for in the prompt, now we need a matching Serial code

In the disassembler `*m_license` is the Name property and `m_license[1]` is the Serial property

The `check()` functions runs `validate(Name, Serial)` which is what we need to focus on

The serial number is corect if `local_24 == local_28` or

```
(sum * 19 + 55) % 127; == ((uint)*(byte *)((long)i + (long)hashed_serial) * 55 + 19) % 127;
```

for 3 different sums of multiples of 3 of the chars of the name: 

`[0],[3],[6], ...`, `[1],[4],[7], ...`, `[2],[5],[8], ...`

I quickly made a python script which simulates the validate function to make our lives easier

```python
import hashlib

def validate(Name, Serial):
    hashed_serial = hashlib.sha256(Serial.encode()).digest()
    success = 1
    
    for i in range(3):
        sum_val = 0
        i_2 = i
        
        while True:
            length = len(Name)
            if length <= i_2:
                break
            sum_val += ord(Name[i_2])
            i_2 += 3
        
        local_24 = (sum_val * 19 + 55) % 127
        local_28 = (hashed_serial[i] * 55 + 19) % 127
        success &= local_24 == local_28
    
    return success

Name = "Walter White Junior"
Serial = "1d117c5a-297d-4ce6-9186-d4b84fb7f230"

s = validate(Name, Serial)
print(s)
```

I then simply made a solution script which bruteforces for a valid serial key

```python
def generate_valid_serial(name):
    while True:
        serial = ''.join(random.choices(string.ascii_lowercase + string.digits + '-', k=36))
        if validate(name, serial):
            return serial

# validate function

name = "Walter White Junior"
valid_serial = generate_valid_serial(name)
print("Generated Valid Serial:", valid_serial)
```

I made the script automatically format the license:
```python
encoded = base64.b64encode(f"Name: {name}\nSerial: {valid_serial}\nType: 1337\n".encode("utf-8")).decode()

license = "----BEGIN WHITE LICENSE----\n" + str(encoded) + "\n-----END WHITE LICENSE-----\n"
```

now I simply need to make the script automate the process of fetching names and submitting results, because I don't want to sit around doing this for 10 hours :-)

Here is the final script:

```python
import hashlib
import random
import string
import base64
import socket
import re

def generate_valid_serial(name):
    while True:
        serial = ''.join(random.choices(string.ascii_lowercase + string.digits + '-', k=36))
        if validate(name, serial):
            return serial

def validate(name, serial):
    hashed_serial = hashlib.sha256(serial.encode()).digest()
    success = 1
    
    for i in range(3):
        sum_val = 0
        i_2 = i
        
        while True:
            length = len(name)
            if length <= i_2:
                break
            sum_val += ord(name[i_2])
            i_2 += 3
        
        local_24 = (sum_val * 19 + 55) % 127
        local_28 = (hashed_serial[i] * 55 + 19) % 127
        success &= local_24 == local_28
    
    return success

def generate_license(name):
  valid_serial = generate_valid_serial(name)

  # Format the license

  encoded = base64.b64encode(f"Name: {name}\nSerial: {valid_serial}\nType: 1337\n".encode("utf-8")).decode()

  license = "----BEGIN WHITE LICENSE----\n" + str(encoded) + "\n-----END WHITE LICENSE-----\n"
  return license


## Interact with netcat

# Define the netcat server address and port
HOST = 'challenges.france-cybersecurity-challenge.fr'
PORT = 2250

def extract_username(message):
    # Use regular expression to extract username from the message
    match = re.search(r'username: (.+)', message)
    if match:
        return match.group(1)
    else:
        return None

def interact_with_netcat():

  with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.connect((HOST, PORT))

      # ignore first line
      data = s.recv(56).decode('utf-8')

      n = 0
      while True:

          data = s.recv(1024).decode('utf-8')
          print(data)

          username = extract_username(data)
          
          if username:
            n+=1
            print(f"\n[{n}/50]...generating license for \"{username}\"...\n")
            license_key = generate_license(username) + "\n"
            
            # send license + newline for submit
            s.sendall(license_key.encode('utf-8'))
            print("Sent:", license_key)
          else:
            print("Failed to extract username.")
            break

interact_with_netcat()
```

After running through all 50 it gives us the flag:

```FCSC{9600becd765ca03baa623504244e008e6f3348bce0219fe4027432d790dd074f}```