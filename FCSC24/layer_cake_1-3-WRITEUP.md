# FCSC24 ~ Layer Cake 1/3

## Challenge Description

- **Title:** 
- **Category:** intro, forensics, docker
- **Points:** 20

```
Un développeur de GoodCorp souhaite publier une nouvelle image Docker. Il utilise une variable d'environnement stockant un flag au moment du build, et vous assure que ce secret n'est pas visible du public. L'image est anssi/fcsc2024-forensics-layer-cake-1.

Récupérez ce flag et prouvez-lui le contraire.
```

## Understanding the Challenge

This challenge gives us a link to a docker image: `https://hub.docker.com/r/anssi/fcsc2024-forensics-layer-cake-1`

An enviroment variable containing the flag was used during the build

## Solution

Looking at the image layers at `https://hub.docker.com/layers/anssi/fcsc2024-forensics-layer-cake-1/latest/images/sha256-e076eb7bc9ef18441fef7e73a08a305c5a1b631dd6789d0fc4f75c25d8c225b3?context=explore` we find the `ARG FIRST_FLAG` which is equal to the flag

```FCSC{a1240d90ebeed7c6c422969ee529cc3e1046a3cf337efe51432e49b1a27c6ad2}```