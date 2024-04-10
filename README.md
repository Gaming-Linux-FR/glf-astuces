## **Table des Matières**

1. **[Problème de Connexion avec un Appareil Bluetooth](#problème-de-connexion-avec-un-appareil-bluetooth)**
2. **[Discord Demande une Mise à Jour non Disponible dans le Répertoire](#discord-demande-une-mise-à-jour-non-disponible-dans-le-répertoire)**
3. **[Configuration du Multiboot avec grub](#configuration-du-multiboot-avec-grub)**
4. **[Accèder à un disque secondaire avec une application **FLATPAK**](#accèder-à-un-disque-secondaire-avec-une-application-flatpak)**
5. **[Comment créer une clé bootable depuis Windows](#comment-créer-une-clé-bootable-depuis-windows)**

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
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
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
