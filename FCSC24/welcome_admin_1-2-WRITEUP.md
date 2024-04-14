# FCSC24 Writeup ~ Welcome Admin 1/2

## Challenge Description

- **Title:** Welcome Admin 1/2
- **Category:** Intro
- **Points:** 20

```
Au coeur d'un réseau labyrinthique, là où la lumière des écrans peine à éclairer les recoins les plus sombres, une demande spéciale est lancée dans les abîmes, un appel discret, attendu seulement par ceux qui connaissent les profondeurs. Seul un véritable expert pourra répondre à l'appel, cryptiquement formulé : "Un expert en SQL est demandé à la caisse numéro 3."
```

## Understanding the Challenge

This challenge gives us 1 file:

```
welcome-admin.tar.xz: XZ compressed data, checksum CRC64
```

which gives us a directory of a **docker project** containing 2 files and a directory when extracted:
```
docker-compose.yml: ASCII text
Dockerfile:         ASCII text
src:                directory
```

and a link: `https://welcome-admin.france-cybersecurity-challenge.fr/`, which points to an admin login on a web page

The website seems to use `SQL` for managing the database, and the challenge description also mentions `SQL` this already makes me think this challenge revolves around an `SQL injection`

## Solution

navigating to `./welcome-admin/src` shows us 1 file and directory:
```
templates:        directory
welcome-admin.py: Python script, ASCII text executable
```

opening the python file with `subl` shows us the websites database checks

Looking at the code validates my previous theory and I run a primitive sql injection
```' OR '1'='1```

which gives us the flag:
```FCSC{94738150696e2903c924f0079bd95cd8256c648314654f32d6aaa090846a8af5}```