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

# Sauvegarde RPI avec compression
```bash
#!/bin/bash
# script to backup Pi SD card
# 2017-06-05
# 2018-11-29    optional name
# DSK='disk4'   # manual set disk
OUTDIR=~/temp/Pi
mkdir -p ~/temp/Pi
# Find disk with Linux partition (works for Raspbian)
# Modified for PINN/NOOBS
export DSK=`diskutil list | grep "Linux" | sed 's/.*\(disk[0-9]\).*/\1/' | uniq`
if [ $DSK ]; then
    echo $DSK
    echo $OUTDIR
else
    echo "Disk not found"
    exit
fi

if [ $# -eq 0 ] ; then
    BACKUPNAME='Pi'
else
    BACKUPNAME=$1
fi
BACKUPNAME+="back"
echo $BACKUPNAME

diskutil unmountDisk /dev/$DSK
echo please wait - This takes some time
echo Ctl+T to show progress!
time sudo dd if=/dev/r$DSK bs=4m | gzip -9 > $OUTDIR/Piback.img.gz

#rename to current date
echo compressing completed - now renaming
mv -n $OUTDIR/Piback.img.gz $OUTDIR/$BACKUPNAME`date +%Y%m%d`.img.gz
```

# Sauvegarde/Restauration MySQL
**Sauvegarde de la base**
```bash
mysqldump -h host -u user -ppass base_de_donnees > fichier_dump
```
**Restauration de la base**
```bash
mysql -h host -u user -ppass base_de_donnees < fichier_dump
```

# Syncronisation entre 2 serveurs
**Génération de la clé SSH (si aucune clé sur le serveur)**
```bash
ssh-keygen -t rsa -b 2048
```
**On copie la clé sur le serveur distant.**
```bash
ssh-copy-id -i /root/.ssh/id_rsa.pub root@serveurdistant
```
**Une fois la clé installé, on obtient le message suivant :**
```bash
Number of key(s) added: 1

Now try logging into the machine, with: "ssh 'root@xx.xx.xx.xx'"
and check to make sure that only the key(s) you wanted were added.
```
**Commande RSYNC**
```bash
rsync -avz -e "ssh -i /root/.ssh/id_rsa" source/ login@serveur:destination/
```
