# <p align="center">Post Installation de Fedora 39</p>

# !!! WIP - Guide en cours d'ecriture et de correction !!!
Petit guide de post installation de Fedora 39.
### *Disclaimer:*
*Il s'agit d'une post-installation adapté à mon utilisation personel, les logiciels et extentions sont à adapter selon l'utilisateur*
    
## 🧐 Sommaire   
- Modification du fichier de configuration DNF, des paquets RPM et mise aàjour
- Installation de ZSH
- Installation de Flathub
- Installation des logiciels
- Bonus: Installation de Topgrade
## 🛠️ 1 - Modification du fichier de configuration DNF, des paquets RPM et mise à jour

DNF est le système de gestion de paquet sur les distros linux basées sur RPM (RHEL, Fedora, Rocky Linux, OpenSUSE, etc.)

Avant toute utilisation, nous allons modifier le fichier de conf DNF afin de le rendre plus rapide (en effet, le gestionnaire de paquet de Fedora peut-être lent)

Pour cela nous allons ouvrir l'éditeur de texte (avec les droits sudo)
```bash
sudoedit /etc/dnf/dnf.conf
```
Une fois que Nano affiche le fichier, on va insérer deux lignes de texte à la fin du fichier. La première permet de lancer le télechargement de paquet en simultanés (entre 3 et 20), la deuxième permet de trouver le miroir de télechargement le plus rapide.
```bash
max_parallel_downloads=10
fastestmirror=True
```
Après la sauvegarde (ctrl+o) et la fermeture du fichier (ctrl+x) nous allons ajouter les paquets RPM Fusion.
Les paquets RPM Fusion sont geré par la communauté et permet d'ajouter des paquets supplémentaires(dont les non-libres).
```bash
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

maintenant nous allons mettre à jour le système
```bash
sudo dnf update && sudo dnf upgrade --refresh && sudo dnf groupupdate core
# pour les mise à jour future seul 'sudo dnf update' est requis
```
Ensuite nous allons mettre à jour les firmwares (sous reserve que les fabriquants supporte les paquets linux)
```bash
sudo fwupdmgr refresh --force
sudo fwupdmgr get-updates
sudo fwupdmgr update
```

## 🛠️ 2 - Installation de ZSH
ZSH(pour Zshell) est un interpreteur de commande (shell), qui est plus flexible et légers que Bash, il permet une meilleur interactivité avec l'utilisateur, notamment avec une correction orthographique et de l'autocompletion.
```bash
sudo dnf install zsh
chsh -s $(wich zsh)
```
Nous allons aussi installer [oh-my-zsh](https://ohmyz.sh/), qui est un framework communautaire qui permet de manager la conf de ZSH et qui contient des centaines de [themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) et de [plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins)
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 🛠️ 3 - Installation de Flathub

Flatpak est un gestionnaire de paquet qui permet de deployer des applications indépendament de la distribution Linux, en mode 'sandbox' et contenant toute les dépendences. [Flathub](https://flathub.org/home) est un magasin d'application qui contient des centaines d'appli.
Flatpak est installé par defaut dans Fedora, mais pas flathub, donc nous allons l'ajouter de cette façon:
```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
reboot
```
## 🛠️ 4 - Installation des logiciels
Nous allons maintenant procéder à l'installation des logiciels, la pluparts proviennent des depos (RPM et Flathub) d'autre vont etre installé depuis Github ou ajout des depots depuis les sites officiel.

#### pour RPM
```bash
sudo dnf install [nom du paquet]
```
pour mon usage, j'utilise les logiciel suivant
| Logiciel | Description                |
| :-------- | :------------------------- |
| `Gnome-Extention`   | personalisation de Gnome   |
| `Transsmission`  | Client torrent  |
| `Tilix`| Terminal multi fenêtres |
| `Gimp` | manipulation d'image |
| `Krita` | Logiciel de dessin vectoriel|
| `Docker`| conteneur applicatif|
| `Clapper`| Lecteur Vidéo|
| `Lutris`| Manageur de Jeux|
| `Steam`| Client de jeux|
| `Chromium`| Navigateur internet|
|`Blender` | infographie 3D|

#### Pour Flatpak
```bash
flatpak install [nom du paquet]
```
| Logiciel | Description                |
| :-------- | :------------------------- |
| `VLC` | Lecteur multimedia|
| `Spotify`| Streaming musical|
| `Visual Studio Code`| Editeur de texte|
| `Gitkraken`| Client Git et Github|
| `Godot` | Moteur de jeux |

#### Autre
| Logiciel | Description                |
| :-------- | :------------------------- |
|`Unity`|Moteur de jeux|

```bash
#ajout du repo Unity Hub à RPM
sudo sh -c 'echo -e "[unityhub]\nname=Unity Hub\nbaseurl=https://hub.unity3d.com/linux/repos/rpm/stable\nenabled=1\ngpgcheck=1\ngpgkey=https://hub.unity3d.com/linux/repos/rpm/stable/repodata/repomd.xml.key\nrepo_gpgcheck=1" > /etc/yum.repos.d/unityhub.repo'
#installation
sudo dnf check-update
sudo dnf install unityhub
```
| Logiciel | Description                |
| :-------- | :------------------------- |
|`libdvdcss`|"pilote pour DVD|

## Bonus: Topgrade

Topgrade est un script qui permet d'automatiser les mises à jour de DNF, flatpak, Snap, mais aussi de Toolbx, Distrobox, Docker, Firmware, Ho-My-ZSH, Cargo, Pip, Extention Gnome et VS Code.

La façon la plus simple pour l'installer sur Fedora est de l'installer via Cargo:

```bash
sudo dnf install cargo
path=%path%;/home/shaggy404/.cargo/bin
cargo install topgrade
sudo cargo install topgrade
export PATH=$PATH:~/.cargo/bin\
export PATH=$PATH:/root/.cargo/bin\
cargo install cargo-update
```

Pour ensuite lancer le script, il suffit de lancer la commande suivante (Le mot de passe super-user est requis):

```bash
topgrade
```

## Contribution
Ce document est libre à toute contributions afin de l'améliorer et de le corriger.