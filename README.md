## **Table des Matières**

1. **[Problème de Connexion avec un Appareil Bluetooth](#problème-de-connexion-avec-un-appareil-bluetooth)**
2. **[Discord Demande une Mise à Jour non Disponible dans le Répertoire](#discord-demande-une-mise-à-jour-non-disponible-dans-le-répertoire)**
3. **[Configuration du Multiboot avec grub](#configuration-du-multiboot-avec-grub)**
4. **[Accès à un second disque sur Steam **FLATPAK**](#accès-à-un-second-disque-sur-steam-flatpak)**

---

<a name="problème-de-connexion-avec-un-appareil-bluetooth"></a>
## Problème de Connexion avec un Appareil Bluetooth

Si vous rencontrez des problèmes de connexion avec un appareil Bluetooth, essayez la manipulation suivante :

```bash
sudo nano /etc/bluetooth/main.conf
```

Modifiez la ligne `#ControllerMode = dual` en `ControllerMode = bredr` puis redémarrez.

---

<a name="discord-demande-une-mise-à-jour-non-disponible-dans-le-répertoire"></a>
## Discord Demande une Mise à Jour non Disponible dans le Répertoire

Discord refusera de se lancer s'il détecte qu'une mise à jour est disponible, affichant le message suivant : "Must be your lucky day, there's a new update !" (C'est votre jour de chance, une nouvelle mise à jour est disponible !). Si la version mise à jour n'est pas encore disponible dans les répertoires officiels de votre distribusion vous pouvez soit utiliser les flatpak, soit désactiver la vérification de mise à jour.

Pour désactiver la vérification de mise à jour, ajoutez la ligne suivante à `~/.config/discord/settings.json` :

```bash
sudo nano ~/.config/discord/settings.json
```

```json
"SKIP_HOST_UPDATE": true
```

Notez que vous devrez ajouter une virgule supplémentaire après l'objet `WINDOW_BOUNDS` en raison des exigences du format JSON, par exemple :

```json
{
  "IS_MAXIMIZED": true,
  "IS_MINIMIZED": false,
  "WINDOW_BOUNDS": {
    "x": 2240,
    "y": 219,
    "width": 1280,
    "height": 720
  },
  "SKIP_HOST_UPDATE": true
}
```

---

## Configuration du Multiboot avec grub

#### Introduction

Le multiboot est un moyen de démarrer plusieurs systèmes d'exploitation sur un même ordinateur. Dans ce tutoriel, nous allons utiliser GRUB, le gestionnaire de démarrage standard pour de nombreuses distribusions Linux, pour configurer un multiboot.

1. **Modifier la Configuration de GRUB** :

   Ouvrez un terminal et exécutez la commande suivante pour ouvrir le fichier de configuration de GRUB :
   ```bash
   sudo nano /etc/default/grub
   ```

Recherchez la ligne contenant `# GRUB_DISABLE_OS_PROBER=false` et supprimez le caractère `#` au début de la ligne pour activer la détection automatique d'autres systèmes d'exploitation.

Si vous voulez également que votre grub mémorise le dernier OS lancé, remplacez la ligne `GRUB_DEFAULT=0` par `GRUB_DEFAULT=saved` et ajoutez `GRUB_SAVEDEFAULT="true"`

   Enregistrez les modifications et quittez l'éditeur de texte.

3. **Installer `os-prober`** :

   Utilisez votre gestionnaire de paquets pour installer `os-prober`, un utilitaire qui permet à GRUB de détecter d'autres systèmes d'exploitation :
   ```bash
   sudo pacman -S os-prober
   ```

4. **Exécuter `os-prober`** :

   Exécutez `os-prober` pour rechercher d'autres systèmes d'exploitation installés sur votre ordinateur :
   ```bash
   sudo os-prober
   ```

5. **Générer la Configuration de GRUB** :

   Utilisez la commande suivante pour générer la configuration de GRUB basée sur les résultats de `os-prober` :
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

## Accès à un second disque sur Steam **FLATPAK**

Si vous souhaitez ajouter un second disque dur ou SSD pour votre bibliothèque de jeux Steam, vous devez accorder à Steam l'accès à ce disque via une commande Flatpak :
              
```bash
flatpak override --user --filesystem=/chemin/vers/votre/Bibliothèque/Steam com.valvesoftware.Steam
```
   
Remplacez `/chemin/vers/votre/Bibliothèque/Steam` par le chemin réel vers votre dossier de bibliothèque Steam sur le second disque.
