# Promenade binaire

### Catégorie

Steganography

### Description

Notre équipe de h4ck3r s'est introduite à la maison blanche et est parvenue à exfiltrer un document PDF assez suspect. Nous pensons qu'il contient une information cruciale sur les goûts alimentaires du nouveau président. Nous comptons sur vous pour la déchiffrer et nous aider à faire pression.
### Fichier

[SteganoPDF.pdf](SteganoPDF.pdf)

### Auteur

Thib

### Solution

C'est un challenge en plusieurs étapes : 

1- En regardant le titre du challenge, on se doute qu'il va falloir utiliser binwalk. On ajoute la paramètre -e pour extraire les données.
```
> binwalk -e SteganoPDF.pdf
```
![Binwalk](data/binwalk.png) 

Ainsi, on voit qu'il y a un PNG, et une archive. Mais seule l'archive a été extraite. Essayons de voir comment extraire le PNG.

2- Il y a plusieurs mannières de faire : 

Avec dd :
```
> dd skip=70 bs=1 if=SteganoPDF.pdf of=Decode.png
```
Avec binwalk :
```
> binwalk -e --dd=".*" SteganoPDF.pdf (solution proposé par @GnousRick sur twitter)
```
Avec Foremost :
```
> foremost SteganoPDF.pdf
```

3- Foremost nous sort 3 fichiers : 

![Foremost](Steganography/PromenadeBinaire/data/foremost_extract.png)

4- Avec Stegsolve (https://github.com/eugenekolo/sec-tools/tree/master/stego/stegsolve/stegsolve) on applique un filtre : 

![Stegsolve](Steganography/PromenadeBinaire/data/stegsolve.png)

```
S0M3_FFF
```

5- On utilise ce mot de passe pour ouvrir l'archive. On tombe sur un document HTML.

6- A la fin du document, on trouve un message suspect :

![B64](Steganography/PromenadeBinaire/data/b64.png)

7- On reconnait la base64 et on le déchiffre :

![Flag](Steganography/PromenadeBinaire/data/flag.png)


### Flag

Hero{P1ZZ4_HWN}
