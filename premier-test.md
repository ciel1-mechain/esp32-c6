![ESP32](https://img.shields.io/badge/ESP32-project-green?logo=espressif&logoColor=white)
 
# Premier code pour la carte ESP32-C6

![vue de dessus](https://espressif-docs.readthedocs-hosted.com/projects/esp-dev-kits/en/latest/_images/esp32-c6-devkitc-1-v1-annotated-photo.png)



# Premiers pas avec la carte ESP32-C6 DevKitC-1 V1

Ce guide vous accompagnera dans la prise en main de la carte ESP32-C6 DevKitC-1 V1.  
Il couvre l’installation des outils, la connexion de la carte et le téléchargement d’un premier programme.

---

## 1. Matériel nécessaire

- Carte ESP32-C6 DevKitC-1 V1
- Câble USB-C
- Ordinateur (Windows, macOS ou Linux)
- Connexion Internet

---

## 2. Installation de l’IDE et des outils

### a) Installer Visual Studio Code (recommandé)
- Télécharger depuis [code.visualstudio.com](https://code.visualstudio.com/)
- Installer l’extension **PlatformIO** ou **ESP-IDF** (Espressif)

### b) Installer l’ESP-IDF (Espressif IoT Development Framework)
- Suivre le guide officiel : [Getting Started ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/get-started/index.html)
- Téléchargez l’installateur pour Windows ou utilisez les scripts pour macOS/Linux :
  - Windows : [ESP-IDF Tools Installer](https://dl.espressif.com/dl/esp-idf/?idf=latest)
  - macOS/Linux :  
    ```bash
    git clone --recursive https://github.com/espressif/esp-idf.git
    cd esp-idf
    ./install.sh esp32c6
    ```

### c) Installer les pilotes USB (si besoin)
- Windows :  
  [Pilotes USB Espressif](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)
- macOS/Linux : Généralement déjà pris en charge.

---

## 3. Connexion de la carte

- Connectez la carte ESP32-C6 à votre ordinateur via le câble USB-C.
- Vérifiez que la LED d’alimentation s’allume.
- Notez le port série attribué :
  - **Windows** : Gestionnaire de périphériques > Ports (COM)
  - **macOS/Linux** :  
    ```bash
    ls /dev/tty.*
    ls /dev/ttyUSB*
    ```

---

## 4. Compilation et flash d’un premier programme

### a) Cloner un projet d’exemple

```bash
git clone https://github.com/espressif/esp-idf-template.git
cd esp-idf-template
```

### b) Configurer l’environnement

- Sous Windows, lancez “ESP-IDF Command Prompt” (installé à l’étape 2).
- Sous macOS/Linux :
  ```bash
  source $HOME/esp/esp-idf/export.sh
  ```

### c) Configurer le projet

```bash
idf.py set-target esp32c6
idf.py menuconfig
```

### d) Compiler et flasher

```bash
idf.py build
idf.py -p <PORT> flash
```
Remplacez `<PORT>` par le port série détecté.

### e) Ouvrir le moniteur série

```bash
idf.py -p <PORT> monitor
```

---

## 5. Ressources utiles

- [Documentation ESP32-C6](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/index.html)
- [Forum Espressif](https://www.esp32.com/)
- [Exemples officiels](https://github.com/espressif/esp-idf/tree/master/examples)

---

**Bonnes expérimentations avec votre ESP32-C6 🚀 !**

---
# Premier programme "Hello World" : Faire clignoter une LED sur ESP32-C6

Ce guide présente un exemple simple pour faire clignoter une LED avec la carte ESP32-C6.  
Ce type de programme est le point de départ classique pour découvrir l’utilisation des broches GPIO.

---

## 1. Connexion matérielle

- Branchez une LED (avec une résistance de 220 à 330 Ω) entre la broche GPIO8 (par exemple) et GND de la carte.
- Le côté long (anode) de la LED va sur GPIO8, le côté court (cathode) sur GND.

> Astuce : Certaines cartes de développement ont déjà une LED connectée (souvent sur GPIO8, GPIO2 ou GPIO5).

---

## 2. Code source (ESP-IDF, C)

Créez un fichier `blink.c` dans le dossier `main/` de votre projet ESP-IDF :

```c
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"

#define LED_GPIO 8 // Modifier selon la broche utilisée

void app_main(void)
{
    gpio_reset_pin(LED_GPIO);
    gpio_set_direction(LED_GPIO, GPIO_MODE_OUTPUT);

    while (1) {
        gpio_set_level(LED_GPIO, 1); // LED ON
        vTaskDelay(500 / portTICK_PERIOD_MS);
        gpio_set_level(LED_GPIO, 0); // LED OFF
        vTaskDelay(500 / portTICK_PERIOD_MS);
    }
}
```

---

## 3. Compilation et flash

Dans un terminal, compilez et téléversez :

```bash
idf.py build
idf.py -p <PORT> flash monitor
```
Remplacez `<PORT>` par le port série de votre carte.

---

## 4. Résultat

La LED doit clignoter à un rythme d’1 seconde (0,5 s allumée, 0,5 s éteinte).

---

**Félicitations, vous avez réalisé votre premier "Hello World" sur ESP32-C6 !**
