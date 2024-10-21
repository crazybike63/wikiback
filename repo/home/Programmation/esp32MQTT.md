---
title: petit serveur web
description: code serveur web avec CHATGPT
published: true
date: 2024-09-05T11:23:09.542Z
tags: 
editor: markdown
dateCreated: 2024-09-05T10:42:10.188Z
---

# ESP32 & MQTT
Apprendre la programmation MQTT sur ESP32 en utilisant le langage C est un excellent projet pour se familiariser avec la communication réseau pour l'Internet des Objets (IoT). L'ESP32 est une plateforme puissante qui peut facilement se connecter à des réseaux Wi-Fi et utiliser des protocoles comme MQTT pour envoyer et recevoir des messages entre des appareils.

Je vais t'accompagner étape par étape pour mettre en place un projet ESP32 avec MQTT en C. Voici les principales étapes à suivre :

## Préparer l'environnement de développement
Avant d'écrire du code, assure-toi d'avoir les bons outils installés.

## Outils nécessaires :
- **IDE** : Visual Studio Code, Arduino IDE, ou PlatformIO.
- **ESP-IDF** : Si tu veux utiliser un environnement professionnel pour C sur l'ESP32, il est recommandé d'installer l'[ESP-IDF (Espressif IoT Development Framework)](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html).
- **Broker MQTT** : Un serveur MQTT pour tester. Tu peux utiliser des services comme **Mosquitto** ou des services en ligne comme **HiveMQ**.

## 1. Installation de l'ESP-IDF :
1. Suis le [guide d'installation officiel](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html) pour ton système d'exploitation.
2. Installe les dépendances et configure l’environnement pour compiler les projets C pour ESP32.

## 2. Configurer MQTT avec l'ESP32
MQTT (Message Queuing Telemetry Transport) est un protocole léger pour la communication machine-to-machine (M2M). Il fonctionne sur un modèle **publish/subscribe**, où un appareil peut publier ou s’abonner à des messages.

### Étape 1 : Créer un projet ESP32

Une fois que tu as installé l'ESP-IDF, tu peux créer un nouveau projet avec la commande suivante (en terminal) :
```bash
idf.py create-project mqtt_esp32
```

### Étape 2 : Importer les librairies MQTT

L'ESP-IDF fournit un support MQTT natif. Pour ajouter MQTT à ton projet, tu dois inclure la librairie MQTT dans ton projet. Voici comment le faire dans `CMakeLists.txt` :

```cmake
set(COMPONENT_REQUIRES esp-mqtt)
```

### Étape 3 : Code MQTT en C pour l'ESP32

Voici un exemple de code pour connecter un ESP32 à un broker MQTT et publier un message :

```c
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_log.h"
#include "mqtt_client.h"
#include "esp_event.h"
#include "nvs_flash.h"

static const char *TAG = "MQTT_EXAMPLE";

static void mqtt_event_handler(void *handler_args, esp_event_base_t base, int32_t event_id, void *event_data) {
    esp_mqtt_event_handle_t event = event_data;
    switch (event->event_id) {
        case MQTT_EVENT_CONNECTED:
            ESP_LOGI(TAG, "MQTT_EVENT_CONNECTED");
            esp_mqtt_client_publish(event->client, "/topic/test", "Hello World!", 0, 1, 0);
            break;
        case MQTT_EVENT_DISCONNECTED:
            ESP_LOGI(TAG, "MQTT_EVENT_DISCONNECTED");
            break;
        case MQTT_EVENT_DATA:
            ESP_LOGI(TAG, "MQTT_EVENT_DATA");
            printf("TOPIC=%.*s\r\n", event->topic_len, event->topic);
            printf("DATA=%.*s\r\n", event->data_len, event->data);
            break;
        default:
            break;
    }
}

void mqtt_app_start(void) {
    esp_mqtt_client_config_t mqtt_cfg = {
        .uri = "mqtt://broker.hivemq.com",  // Le broker MQTT à utiliser
    };

    esp_mqtt_client_handle_t client = esp_mqtt_client_init(&mqtt_cfg);
    esp_mqtt_client_register_event(client, ESP_EVENT_ANY_ID, mqtt_event_handler, client);
    esp_mqtt_client_start(client);
}

void app_main(void) {
    esp_err_t ret = nvs_flash_init();
    if (ret == ESP_ERR_NVS_NO_FREE_PAGES || ret == ESP_ERR_NVS_NEW_VERSION_FOUND) {
        ESP_ERROR_CHECK(nvs_flash_erase());
        ret = nvs_flash_init();
    }
    ESP_ERROR_CHECK(ret);

    ESP_LOGI(TAG, "ESP32 MQTT Client Example");
    mqtt_app_start();
}
```

### Explication du code :
1. **Initialisation du client MQTT** :
   Le client est configuré avec un broker public (HiveMQ dans cet exemple). Tu peux changer `mqtt://broker.hivemq.com` par l'adresse de ton propre serveur MQTT.

2. **Gestion des événements** :
   - `MQTT_EVENT_CONNECTED` : Lorsque l'ESP32 est connecté au broker, il publie un message sur le topic `/topic/test`.
   - `MQTT_EVENT_DATA` : Lorsque l'ESP32 reçoit des données, il affiche le topic et les données reçues.
  
3. **Fonction principale `app_main`** :
   C’est le point d’entrée de l'application ESP32. Elle initialise le stockage NVS (pour le Wi-Fi et autres configurations), puis lance le client MQTT.

###  3.Flasher le code sur l'ESP32

1. Connecte ton ESP32 via USB à ton ordinateur.
2. Utilise `idf.py build` pour compiler le projet.
3. Une fois la compilation terminée, flashe le firmware sur l’ESP32 avec :
   ```bash
   idf.py flash
   ```

4. Laisse l’ESP32 redémarrer et observe les logs via :
   ```bash
   idf.py monitor
   ```

### 4.Tester avec un broker MQTT

Pour tester l'envoi et la réception des messages :
- Utilise un **client MQTT** comme **MQTT.fx** ou **MQTT Explorer**.
- Abonne-toi au topic `/topic/test` sur ton client pour voir les messages envoyés par l'ESP32.

###  5.Améliorations possibles

Une fois que tu maîtrises les bases :
- Ajoute la connexion sécurisée (SSL/TLS) à ton client MQTT.
- Abonne l'ESP32 à plusieurs topics et gère les événements MQTT.
- Crée des capteurs IoT qui envoient des données (comme la température) via MQTT à une base de données.

---

Si tu as des questions ou si tu veux approfondir un aspect particulier, je peux t'aider plus en détail !
Christophe MONTMEAT