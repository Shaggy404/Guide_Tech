

# <p align="center">VHD Windows 10 depuis Linux
</p>
  Guide de création d'une VHD Windows 10 depuis une distribution GNU/Linux (Fedora 47 Workstation) et utilisation sur une clé USB 3.1 Samsung 64Gig et Ventoy
    

## 🛠️ Outils utilisé
- [ISO Windows 10](https://www.microsoft.com/en-us/software-download/windows10ISO) 
- [Ventoy](https://www.ventoy.net/en/download.html)
- [ventoy_vhdboot.img](https://www.ventoy.net/en/plugin_vhdboot.html)
- [KVM/QEMU](https://www.qemu.org/)
- [Gparted](https://gparted.org/)


## 🧐 Guide d'installation    
- Il faut installer dans un premier temps installer KVM/QEMU sur Fedora
```bash
# Installation de base
sudo dnf -y install bridge-utils libvirt virt-install qemu-kvm

# Installation des outils complémentaire
sudo dnf install libvirt-devel virt-top libguestfs-tools guestfs-tools

# Lancement du daemon KVM et activation au démarrage
sudo systemctl start libvirtd
sudo systemctl enable libvirtd

# installation de l'interface graphique(GUI) du manager de VM
sudo dnf -y install virt-manager
```

- Lancer le terminal et ecrire la commande suivante pour crée le disque virtuel
```bash
#Création du fichier .vhd (40G correspond à la taille)
qemu-img create -f vpc windows.vhd 40G\
```
- créer une nouvelle machine virtuelle (manuellement) dans Virt-Manager, laisser les paramètres mémoire et CPU par défaut, puis sélectionner le VHD dans la sélection de stockage personalisé. Ensuite personalisé avant l'installation et ajouter l'ISO de Windows en péripherique de stockage CD/DVD et le mettre en premier dans les options de démarrage. (!!! Pensez aussi à désactiver l'interface réseau pendant l'installation pour ne pas à utiliser de compte Windows et d'utiliser un compte local !!!)
- Lancer l'installation de Windows et preparer la clé avec Ventoy avec les commande suivante
```bash
cd ~/Downloads/

#Décompression du ficher .tar.gz
tar xvf ventoy-1.x.xx-linux.tar.gz

# Lancement du script d'installation en mode graphique
./VentoyGUI.x86_64

# Ou lancement en ligne de commande(/dev/sdX correspond à votre clé USB)
sh Ventoy2Disk.sh { -i | -I | -u } /dev/sdX
```
- Ventoy va crée deux partition sur la clé USB, une "VTOYEFI" qui n'apparait pas dans l'explorateur de fichier et une partition "Ventoy" sur laquelle sont installé les ISOs et IMGs.
- La partition "Ventoy" est au format ext4 et pour que le VHD se lance, il faut la formater au format ntfs. Pour cela la façon la plus simple est d'utiliser Gparted (disponible pour la plupart des distribution Linux)
- Une fois fait, il faut crée un dossier "ventoy" (attention à la casse) dans la partition et y mettre le fichier ventoy_vhdboot.img
- Une fois l'installation de Windows terminé, éteindre la VM, quitter KVM et copier le VHD dans Ventoy/ventoy
- Redémarrer le poste hôte, booter sur la clé et sélectionner le VHD


## 🙇 Auteur
#### thibault DEVEMY
- Github: [Shaggy404](https://github.com/Shaggy404)    
        
        


        
        
        



        
        
