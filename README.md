# Sauvegarde/Restauration carte SD
**Sauvegarde de la carte SD à partir d'un Mac**
```bash
sudo dd bs=4m if=/dev/disk2 | gzip > raspbian.img.gz
```
**Restauration de l'image sur la carte SD**
```bash
gunzip --stdout raspbian.img.gz | sudo dd bs=4m of=/dev/disk2
```
> Sous linux le BS=4m s'écrit avec un M majuscule
