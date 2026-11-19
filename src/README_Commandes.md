# STM32 MQTT Sensors

Publication des données de capteurs d'une carte STM32 via MQTT (Mosquitto), avec visualisation sous Node-RED.

> **Note** : toutes les commandes se lancent dans un terminal `cmd` **en tant qu'administrateur**.
> Remplacez `<USER>` / `<PASSWORD>` par vos identifiants Mosquitto et adaptez les adresses IP à votre réseau.

## Sommaire

1. [Lancer Mosquitto](#1-lancer-mosquitto)
2. [Tests](#2-tests)
3. [Affichage des valeurs des topics](#3-affichage-des-valeurs-des-topics)
4. [Lancer Node-RED](#4-lancer-node-red)

---

## 1. Lancer Mosquitto

**Terminal 1 :**

```bat
cd C:\mosquitto
mosquitto -c mosquitto.conf -v
```

Ouvrir ensuite le fichier `stm32_mqtt_sensors.ino` et exécuter le code (téléversement sur la carte).

---

## 2. Tests

### 2.1 Vérification réseau

```bat
cd C:\mosquitto
netstat -an | findstr :1883
ipconfig
ping 192.168.0.135
ping 192.168.0.22
```

| Commande | Rôle |
|----------|------|
| `netstat -an \| findstr :1883` | Vérifier que Mosquitto écoute sur le port 1883 (doit afficher `TCP 0.0.0.0:1883 ... LISTENING`) |
| `ping 192.168.0.135` | Adresse IP WiFi du PC (broker local) |
| `ping 192.168.0.22` | Adresse IP de la carte |

### 2.2 Test publication / abonnement

**Terminal 1 (abonné) :**

```bat
cd C:\mosquitto
mosquitto_sub -h 192.168.0.135 -t "/test" -u <USER> -P <PASSWORD>
```

**Terminal 2 (publieur) :**

```bat
cd C:\mosquitto
mosquitto_pub -h 192.168.0.135 -t "/test" -m "hello" -u <USER> -P <PASSWORD>
```

**Résultat :** le message `hello` s'affiche sur le **Terminal 1** (l'abonné).

---

## 3. Affichage des valeurs des topics

```bat
cd C:\mosquitto
```

### Tous les topics (broker local)

```bat
mosquitto_sub -h 192.168.0.135 -t "#" -v -u <USER> -P <PASSWORD>
```

### Topics des capteurs uniquement

**Broker local** (`192.168.0.135`, adresse IP WiFi du PC) :

```bat
mosquitto_sub -h 192.168.0.135 -t "/temperature" -t "/Humidite" -t "/Pression" -t "/Accelero_X" -t "/Accelero_Y" -t "/Accelero_Z" -t "/Gyro_X" -t "/Gyro_Y" -t "/Gyro_Z" -t "/Magneto_X" -t "/Magneto_Y" -t "/Magneto_Z" -u <USER> -P <PASSWORD>
```

**Broker public** (`broker.hivemq.com` ou `test.mosquitto.org`) :

```bat
mosquitto_sub -h broker.hivemq.com -t "/temperature" -t "/Humidite" -t "/Pression" -t "/Accelero_X" -t "/Accelero_Y" -t "/Accelero_Z" -t "/Gyro_X" -t "/Gyro_Y" -t "/Gyro_Z" -t "/Magneto_X" -t "/Magneto_Y" -t "/Magneto_Z"
```

### Topics disponibles

| Capteur | Topics |
|---------|--------|
| Environnement | `/temperature`, `/Humidite`, `/Pression` |
| Accéléromètre | `/Accelero_X`, `/Accelero_Y`, `/Accelero_Z` |
| Gyroscope | `/Gyro_X`, `/Gyro_Y`, `/Gyro_Z` |
| Magnétomètre | `/Magneto_X`, `/Magneto_Y`, `/Magneto_Z` |

---

## 4. Lancer Node-RED

```bat
cd C:\mosquitto
node-red
```

Ouvrir dans le navigateur :

- Éditeur de flux : <http://127.0.0.1:1880/>
- Dashboard : <http://127.0.0.1:1880/ui>
