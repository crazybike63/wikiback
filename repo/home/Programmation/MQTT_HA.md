---
title: MQTT & Homeassistant
description: 
published: true
date: 2024-09-05T10:47:50.843Z
tags: 
editor: markdown
dateCreated: 2024-09-04T11:08:50.333Z
---

# Header
sendMQTTDiscoveryMsg_global() {
String monGPIO ="gpio14";
.....
...
DeviceBin2Discover(monGPIO);
}


SendDataToHomeAssistant() {
String monGPIO ="gpio14";
.....
...
int etatgpio = digitalRead (14);
sprintf(value, "%s,\"%s\":%d", value, monGPIO.c_str(), etatgpio);
...
}