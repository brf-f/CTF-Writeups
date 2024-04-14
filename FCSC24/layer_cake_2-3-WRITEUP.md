# FCSC24 Writeup ~ Layer Cake 2/3

## Challenge Description

- **Title:** Layer Cake 2/3
- **Category:** intro, forensics, docker
- **Points:** 20

```
Un développeur de GoodCorp souhaite publier une nouvelle image Docker. Il copie au moment du build un fichier contenant un flag, puis le supprime. Il vous assure que ce secret n'est pas visible du public. L'image est anssi/fcsc2024-forensics-layer-cake-2.

Récupérez ce flag et prouvez-lui le contraire.
```

## Understanding the Challenge

This challenge gives us a link to a docker image: `https://hub.docker.com/r/anssi/fcsc2024-forensics-layer-cake-2`

The flag is located in a deleted file

## Solution

Saving as `.tar` file
```docker save -o image.tar IMAGE_NAME:TAG```

extracting `.tar` files, navigate to correct layer, extract `layer.tar`

now just `cat` contents of `/temp/secret`