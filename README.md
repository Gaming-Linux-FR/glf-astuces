## **Table des Matières**

1. **[Problème de Connexion avec un Appareil Bluetooth](#problème-de-connexion-avec-un-appareil-bluetooth)**
2. **[Discord Demande une Mise à Jour non Disponible dans le Répertoire](#discord-demande-une-mise-à-jour-non-disponible-dans-le-répertoire)**
3. **[Configuration du Multiboot avec grub](#configuration-du-multiboot-avec-grub)**
4. **[Accèder à un disque secondaire avec une application **FLATPAK**](#accèder-à-un-disque-secondaire-avec-une-application-flatpak)**
5. **[Comment créer une clé bootable depuis Windows](#comment-créer-une-clé-bootable-depuis-windows)**
6. **[Problème de performance avec CPU Intel](#problème-de-performance-avec-cpu-intel)**

---

<a name="problème-de-connexion-avec-un-appareil-bluetooth"></a>
## Problème de Connexion avec un Appareil Bluetooth

Si vous rencontrez des problèmes de connexion avec un appareil Bluetooth, essayez la manipulation suivante :

```bash
sudo sed -i 's/#ControllerMode = dual/ControllerMode = bredr/' /etc/bluetooth/main.conf && sudo systemctl restart bluetooth
```

---

<a name="discord-demande-une-mise-à-jour-non-disponible-dans-le-répertoire"></a>
## Discord Demande une Mise à Jour non Disponible dans le Répertoire

Discord refusera de se lancer s'il détecte qu'une mise à jour est disponible, affichant le message suivant : "Must be your lucky day, there's a new update !" (C'est votre jour de chance, une nouvelle mise à jour est disponible !). Si la version mise à jour n'est pas encore disponible dans les répertoires officiels de votre distribution vous pouvez soit utiliser les flatpak, soit désactiver la vérification de mise à jour.

Pour désactiver la vérification de mise à jour, ajoutez la ligne suivante à `~/.config/discord/settings.json` :

```bash
nano ~/.config/discord/settings.json
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

Le multiboot est un moyen de démarrer plusieurs systèmes d'exploitation sur un même ordinateur. Dans ce tutoriel, nous allons utiliser GRUB, le gestionnaire de démarrage standard pour de nombreuses distributions Linux, pour configurer un multiboot.

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

   Sur base Arch :
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

  Sur base Fedora : 
  ```bash
  grub2-mkconfig -o /boot/grub2/grub.cfg
   ```

  Sur base Ubuntu : 
  ```bash
  sudo update-grub
  ```

---

## Accèder à un disque secondaire avec une application flatpak

Attention ! Certains Flatpak comme celui de Steam refusent de se lancer si on leur donne accès à tous les dossiers. Il faut donc uniquement donner à Steam Flatpak l'accès aux bibliothèques de jeu.

Le plus simple et d'utiliser Flatseal, on selectionne le flatapk on clique sur ![image](https://github.com/Gaming-Linux-FR/glf-astuces/assets/83916775/20e9afff-149d-4550-8279-189ae5dd1e48).


Et on ajoute le chemin ou vous montez vos disques durs / SSD secondaires. /media est recommandé pour cet usage.

![image](https://github.com/Gaming-Linux-FR/glf-astuces/assets/83916775/c71f8829-557b-4713-8397-a572add5051c)

Il faut fermer Flatseal et redémarrer le Flatpak concerné pour appliquer la modification.

Il faudra relancer l'application Flatpak pour qu'elle puisse détecter le nouveau disque.

---

## Comment créer une clé bootable depuis Windows

Tuto côté Emmabuntüs : https://emmabuntus.org/installer-emmabuntus-de5/#Avec_loutil_Etcher

---

## Problème de performance avec CPU Intel

### Diagnostic du problème :

Si vous rencontrez des chutes importantes de FPS (images par seconde) pendant que vous jouez à un jeu, il est possible que le problème soit lié à la détection de verrouillage fractionné. Vous pouvez vérifier cela en suivant ces étapes :

1. Lancez votre jeu comme d'habitude.
2. Ouvrez un terminal et utilisez la commande suivante pour vérifier les messages système liés à la détection de verrouillage fractionné :
   ```bash
   sudo dmesg | grep split
   ```
3. Si vous voyez une sortie semblable à `[ 338.275160] x86/split lock detection: #AC: GoW.exe/6993 took a split_lock trap at address: 0x140b35229`, alors vous avez probablement le problème de détection de verrouillage fractionné.

### Solutions au problème :

#### Option (a) : Modification des paramètres de démarrage :

Si vous préférez désactiver la détection de verrouillage fractionné au démarrage du système :

```bash
echo "kernel.split_lock_mitigate=0" | sudo tee /etc/sysctl.d/99-split-lock.conf
```
2. Redémarrer.

3. Vérifiez que la détection de verrouillage fractionné est désactivée en utilisant la commande :
   ```bash
   sysctl -a | grep split
   ```

#### Option (b) : Désactivation temporaire à l'aide de sysctl :

Si vous avez une version de noyau Linux égale ou supérieure à 6.2, vous pouvez désactiver temporairement la détection de verrouillage fractionné à l'aide de la commande `sysctl`.  Elle est effective immédiatement mais il faudra la refaire après chaque démarrage. Suivez ces étapes :

1. Avant de lancer le jeu, ouvrez un terminal.
2. Utilisez la commande suivante pour désactiver temporairement la détection de verrouillage fractionné :
   ```bash
   sudo sysctl kernel.split_lock_mitigate=0
   ```
3. Vérifiez que la détection de verrouillage fractionné est désactivée en utilisant la commande :
   ```bash
   sysctl -a | grep split
   ```

### Résultats :

Après avoir désactivé la détection de verrouillage fractionné, vous devriez remarquer une amélioration significative des performances de votre jeu. Si vous avez suivi ces étapes correctement, votre jeu devrait maintenant fonctionner de manière fluide et sans les chutes de FPS précédentes. Sur ma machine, dans par exemple dans God Of War je passe de 40 fps à 150fps.

### Liens utiles :

- [Discussion sur GitHub - Problème de détection de verrouillage fractionné](https://github.com/doitsujin/dxvk/issues/2938)
- [Article sur Phoronix - Impact du verrouillage fractionné sur les jeux Linux](https://www.phoronix.com/news/Linux-Splitlock-Hurts-Gaming)
