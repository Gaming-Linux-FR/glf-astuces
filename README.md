## **Table des Matières**

1. **[Problème de Connexion avec un Appareil Bluetooth](#problème-de-connexion-avec-un-appareil-bluetooth)**
2. **[Discord Demande une Mise à Jour non Disponible dans le Répertoire](#discord-demande-une-mise-à-jour-non-disponible-dans-le-répertoire)**

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

Discord refusera de se lancer s'il détecte qu'une mise à jour est disponible, affichant le message suivant : "Must be your lucky day, there's a new update !" (C'est votre jour de chance, une nouvelle mise à jour est disponible !). Si la version mise à jour n'est pas encore disponible dans les répertoires officiels de votre distrivution vous pouvez soit utiliser les flatpak, soit désactiver la vérification de mise à jour.

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
