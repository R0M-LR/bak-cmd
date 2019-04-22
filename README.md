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

# Sauvegarde/Restauration MySQL
**Sauvegarde de la base**
```bash
mysql -h host -u user -ppass base_de_donnees > fichier_dump
```
**Restauration de la base**
```bash
mysql -h host -u user -ppass base_de_donnees < fichier_dump
```

# Syncronisation de dossiers entre 2 serveurs
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
