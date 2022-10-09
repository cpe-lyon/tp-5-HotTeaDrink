# TP 5 Systèmes de fichiers, partitions et disques

- [TP 5 Systèmes de fichiers, partitions et disques](#tp-5-systèmes-de-fichiers-partitions-et-disques)
  - [Exercice 1 : Disques et partitions](#exercice-1--disques-et-partitions)
  - [Exercice 2 : Partitionnement LVM](#exercice-2--partitionnement-lvm)

## Exercice 1 : Disques et partitions

2. On affiche les disques durs avec la commande suivante:

```BASH
sudo fdisk -l
```

3. Pour créer les partitions de disque il faut se rendre sur le disque avec la commande :

```BASH
sudo fdisk /dev/sdb
```

Puis on tape n qui est la commande permettant de créer les partitions, puis on choisit s'il s'agit d'une partition principale ou non, et suite à ça on choisit la taille de la partition qu'on souhaite.
On réalise la même action pour la deuxième partition.
Pour modifier le type de partitions on utilise la commande t puis on choisit quelle partitions on veut changer et ensuite on indique l'ID.

4. Pour formater les partitions on utilise la commande :

```BASH
sudo mkfs -t ext4 /dev/sd1/2
```

5. La commande ne fonctionne pas car les partitions n'ont pas été monté.

6. On crée tout d'abord les deux dossiers pour les points de montage data et win puis on monte les partitions dans les dossiers respectifs.
Pour les faire monter automatiquement il faut modifier le fichier fstab.

``` BASH
/dev/sdb1 /data ext4 default 0 0 
/dev/sdb2 /win ntfs default 0 0 
 ```

7. Il suffit de tapper les commandes suivantes

```BASH
sudo mount -t ext4 /dev/sdb1 data 
sudo mount -t ext4 /dev/sdb2 win 
```

## Exercice 2 : Partitionnement LVM

2. Suite à un problème de machine virtuelle j'ai du utilisé ma deuxième pour réaliser le deuxième exercice, j'ai donc recrée un disque et ensuite crée la partition de type LVM

3. On crée le volume physique LVM puis on vérifie son existence avec les commandes:

```BASH
sudo pvcreate /dev/sdb1
pvdisplay
```

4. Par la suite le groupe de volume se crée avec la commande suivante:

```BASH
sudo vgcreate vg00 /dev/sdb1
```

5. 

```BASH
lvcreate -l 100%FREE -n lvData vg00
```

6. 

```BASH
mkfs -t ext4 /dev/vg00/lvData1
```

8. On ajoute le nouveau disque au groupe

```BASH
sudo vgextend vg00 /dev/sdc1
```

9. Puis on peut modifier la taille du volume logique :

```BASH
lvextend -L+1G /dev/vg00/lvData
```

Mais tout en modifiant le file system après 

```BASH
resize2fs /dev/vmvg/Vol1
```
