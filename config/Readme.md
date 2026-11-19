# 🔐 Authentification Mosquitto

## 1. Configuration de `mosquitto.conf`

Ajouter ces 3 lignes à la fin du fichier `mosquitto.conf` :

```conf
listener 1883 0.0.0.0
allow_anonymous false
password_file C:\Users\ihebz\Desktop\mosquitto\passwd
```

| Ligne | Rôle |
|-------|------|
| `listener 1883 0.0.0.0` | Écoute sur le port 1883, sur toutes les interfaces réseau |
| `allow_anonymous false` | Refuse les connexions sans authentification |
| `password_file ...` | Utilise le fichier de mots de passe |

## 2. Création du fichier de mots de passe

```bat
mosquitto_passwd -c C:\Users\ihebz\Desktop\mosquitto\passwd iheb
```

Entrer ensuite le mot de passe de l'utilisateur lorsqu'il est demandé (`<PASSWORD>`).

> **Remarque** : l'option `-c` crée le fichier (et écrase un fichier existant). Pour ajouter un autre utilisateur, relancer la commande **sans** `-c`.

> **Sécurité** : ne pas publier le fichier `passwd` sur GitHub (l'ajouter au `.gitignore`).
