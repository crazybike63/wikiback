---
title: programmation esp32 avec chatgpt pour homeassistant
description: 
published: true
date: 2024-09-05T12:26:56.333Z
tags: 
editor: markdown
dateCreated: 2024-09-05T12:01:18.059Z
---

Pour intégrer un ESP32 avec Home Assistant en utilisant MQTT Autodiscovery, tout en fournissant un serveur web affichant des informations comme le niveau de signal Wi-Fi (SSID) et le temps de fonctionnement (uptime) de l'ESP32, voici un guide détaillé pour t'aider à mettre en place ce projet.

# 1. Prérequis
ESP32 avec le firmware Arduino.
Home Assistant configuré avec MQTT.
Un broker MQTT (comme Mosquitto) configuré dans Home Assistant.
Bibliothèques Arduino pour MQTT et Wi-Fi : PubSubClient et WiFi.h.

# 2. Configurer le MQTT Autodiscovery avec Home Assistant
MQTT Autodiscovery permet à Home Assistant de détecter automatiquement les nouveaux périphériques et de les ajouter à l’interface.

Étapes pour l'ESP32 :
Connecte l'ESP32 à ton réseau Wi-Fi.
Utilise le protocole MQTT pour envoyer des informations à Home Assistant.
Publie les messages de découverte au bon format afin que Home Assistant les interprète.
Voici un exemple de code Arduino pour l'ESP32 qui gère MQTT, l'autodiscovery, et un serveur web pour afficher le SSID et l'uptime.

# 3. Code Arduino pour l'ESP32

`#include <WiFi.h>
#include <PubSubClient.h>
#include <ArduinoJson.h>

const char* ssid = "Votre_SSID";
const char* password = "Votre_Mot_de_Passe";
const char* mqtt_server = "Adresse_IP_MQTT_Broker";
const char* device_name = "ESP32_Device";

WiFiClient espClient;
PubSubClient client(espClient);

// Variables pour MQTT
unsigned long lastMsg = 0;
char msg[50];
long uptime = 0;

// Crée un serveur web
WiFiServer server(80);

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Connexion à ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connecté");
  Serial.print("Adresse IP : ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Tentative de connexion au serveur MQTT...");
    if (client.connect(device_name)) {
      Serial.println("connecté");
      // Enregistrement via autodiscovery MQTT
      publish_mqtt_autodiscovery();
    } else {
      Serial.print("échec, rc=");
      Serial.print(client.state());
      Serial.println(" nouvelle tentative dans 5 secondes");
      delay(5000);
    }
  }
}

void publish_mqtt_autodiscovery() {
  // Générer les configurations de l'ESP32 pour l'autodécouverte Home Assistant
  StaticJsonDocument<200> discovery_msg;
  discovery_msg["name"] = "ESP32 Uptime";
  discovery_msg["state_topic"] = String("homeassistant/sensor/") + device_name + "/uptime/state";
  discovery_msg["unit_of_measurement"] = "seconds";
  discovery_msg["device_class"] = "timestamp";

  char buffer[256];
  size_t n = serializeJson(discovery_msg, buffer);
  client.publish(String("homeassistant/sensor/") + device_name + "/uptime/config", buffer, n);
}

void send_status() {
  uptime = millis() / 1000;
  snprintf(msg, 50, "%ld", uptime);
  client.publish(String("homeassistant/sensor/") + device_name + "/uptime/state", msg);
}

void setup() {
  Serial.begin(115200);
  setup_wifi();
  
  client.setServer(mqtt_server, 1883);

  // Démarrer le serveur web
  server.begin();
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  unsigned long now = millis();
  if (now - lastMsg > 5000) {
    lastMsg = now;
    send_status(); // Envoi du temps de fonctionnement (uptime) à Home Assistant
  }

  // Gérer les requêtes du serveur web
  WiFiClient webClient = server.available();
  if (webClient) {
    if (webClient.available()) {
      webClient.println("<html><head><title>ESP32 Info</title></head><body>");
      webClient.println("<h1>ESP32 Web Server</h1>");
      webClient.print("<p>SSID: ");
      webClient.print(WiFi.SSID());
      webClient.println("</p>");
      webClient.print("<p>Uptime: ");
      webClient.print(uptime);
      webClient.println(" seconds</p>");
      webClient.println("</body></html>");
      webClient.stop();
    }
  }
}
`

# 4. Explication du code
Connexion Wi-Fi : Le code se connecte au réseau Wi-Fi via les identifiants fournis.
Serveur MQTT : L'ESP32 se connecte au broker MQTT spécifié.
Autodiscovery MQTT : Le message de découverte est envoyé à Home Assistant pour ajouter automatiquement le capteur.
Serveur Web : Le serveur web intégré affiche des informations sur le réseau et le temps de fonctionnement (uptime).
Envoi des données d'uptime : Toutes les 5 secondes, l'ESP32 publie son uptime sur le broker MQTT, visible dans Home Assistant.

# 5. Configurer Home Assistant
Une fois le projet ESP32 en marche, Home Assistant devrait détecter automatiquement le capteur via MQTT Autodiscovery.
Vérifie dans les paramètres MQTT de Home Assistant pour t'assurer que l'ESP32 est bien ajouté.

# 6. Serveur web
Accède au serveur web de l'ESP32 en tapant son adresse IP locale dans un navigateur. Tu pourras voir le SSID auquel l'ESP32 est connecté et son uptime.