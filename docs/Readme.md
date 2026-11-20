# ESP32-S3-RLCD-4.2 — Station Météo & Domotique ESPHome / Weather & Smart Home Display

[Français](#français) | [English](#english)

---

## Français

Interface monochrome haute lisibilité pour l'écran réflectif **Waveshare ESP32-S3-RLCD-4.2** (`400 × 300 px`, contrôleur `ST7305`) pilotée par **ESPHome** et **Home Assistant**.

### Aperçu de l'interface (400 × 300 px)

L'écran est structuré en quatre zones horizontales optimisées pour la lecture à distance :

| Zone | Coordonnées Y | Contenu affiché |
| :--- | :--- | :--- |
| **1. Barre de statut** | `0 → 40 px` | Icône micro muet (`mute_switch`), titre centré (`ESP32-S3-RLCD-4.2`), force du signal Wi-Fi (RSSI dynamique) |
| **2A. Intérieur (Gauche)** | `40 → 215 px` (`x: 0 → 200`) | Icône maison, température intérieure (`56 px` gras + `°C` dynamique), humidité intérieure (`42 px` gras + icône goutte) |
| **2B. Extérieur (Droite)** | `40 → 215 px` (`x: 200 → 400`) | Icône sapin, température extérieure (`56 px` gras), icône météo actuelle (`42 px`), humidité, vent (vitesse + direction) et prévision suivante (`Ensuite : ...`) |
| **3. Batterie** | `215 → 245 px` | Icône batterie, jauge dynamique calculée automatiquement selon la largeur des textes, tension (`V`) et pourcentage (`%`) |
| **4. Pied de page** | `245 → 300 px` | Heure (`HH:MM`) à gauche, SSID Wi-Fi et adresse IP empilés au centre, date (`JJ/MM/AAAA`) à droite |

### Matériel & Câblage SPI

* **Carte / Écran** : Waveshare ESP32-S3-RLCD-4.2 (`400 × 300`, driver `st7305`)
* **Intervalle de rafraîchissement** : `1 min`

| Signal | Broche ESP32-S3 |
| :--- | :--- |
| `CLK` | `GPIO11` |
| `MOSI` | `GPIO12` |
| `CS` | `GPIO40` |
| `DC` | `GPIO5` |
| `RESET` | `GPIO41` |

### Prérequis & Polices

1. **Polices Google Fonts** (téléchargées automatiquement lors de la compilation) :
   * `Roboto` (`13 px`, `16 px`, `20 px`)
   * `Roboto@700` (`42 px`, `56 px`)
2. **Police d'icônes locale** :
   * Placez le fichier `materialdesignicons-webfont.ttf` dans le répertoire `fonts/` de votre configuration ESPHome (`/config/esphome/fonts/materialdesignicons-webfont.ttf`).

### Entités & Variables ESPHome utilisées

#### Capteurs locaux & système
* `wifi` (`id: wifi_component`), `wifi_signal` (`id: wifi_signal_db`), `wifi_info` (`id: wifi_ssid_sensor`, `id: wifi_ip`)
* `time` (`id: esptime`)
* `switch` (`id: mute_switch`) — État muet du microphone
* `sensor` :
  * `temp_sensor` / `hum_sensor` — Température et humidité intérieures
  * `bat_level` / `bat_voltage` — Pourcentage et tension de la batterie

#### Capteurs Home Assistant (Météo extérieure)
* `ha_weather_temperature` — Température extérieure (`°C`)
* `ha_weather_humidity` — Humidité extérieure (`%`)
* `ha_weather_wind` — Vitesse du vent (`km/h`)
* `ha_weather_next` (`text_sensor`) — Prochaine condition météo annoncée

#### Variables globales de repli (`globals:`)
Utilisées automatiquement si les capteurs ne sont pas encore initialisés au démarrage :
* `gtemp`, `ghum`
* `gweather` (index `0` à `9`), `gweather_temp`, `gweather_hum`, `gweather_wind`, `gweather_wind_dir` (index `0` à `7` : `N`, `NE`, `E`, `SE`, `S`, `SO`, `O`, `NO`), `gweather_next`
* `gbat`, `gvolt`

### Installation

1. Copiez `materialdesignicons-webfont.ttf` dans `esphome/fonts/`.
2. Intégrez les blocs `spi:`, `font:`, `color:` et `display:` dans le fichier YAML de votre appareil.
3. Compilez et téléversez :
   ```bash
   esphome run votre-appareil.yaml
   ```

---

## English

High-contrast monochrome UI for the **Waveshare ESP32-S3-RLCD-4.2** reflective LCD (`400 × 300 px`, `ST7305` controller) powered by **ESPHome** and **Home Assistant**.

### UI Layout Overview (400 × 300 px)

The screen is divided into four horizontal zones designed for high readability from a distance:

| Zone | Y Coordinates | Displayed Content |
| :--- | :--- | :--- |
| **1. Status Bar** | `0 → 40 px` | Microphone mute icon (`mute_switch`), centered title (`ESP32-S3-RLCD-4.2`), dynamic Wi-Fi signal strength (RSSI) |
| **2A. Indoor (Left)** | `40 → 215 px` (`x: 0 → 200`) | Home icon, indoor temperature (`56 px` bold + dynamic `°C`), indoor humidity (`42 px` bold + water droplet icon) |
| **2B. Outdoor (Right)** | `40 → 215 px` (`x: 200 → 400`) | Pine tree icon, outdoor temperature (`56 px` bold), current weather icon (`42 px`), humidity, wind (speed + direction), and next forecast (`Ensuite : ...`) |
| **3. Battery** | `215 → 245 px` | Battery icon, dynamic progress bar auto-sized to fit surrounding text widths, voltage (`V`), and percentage (`%`) |
| **4. Footer** | `245 → 300 px` | Time (`HH:MM`) on the left, stacked Wi-Fi SSID & IP address in the center, date (`DD/MM/YYYY`) on the right |

### Hardware & SPI Wiring

* **Board / Display**: Waveshare ESP32-S3-RLCD-4.2 (`400 × 300`, `st7305` driver)
* **Update Interval**: `1 min`

| Signal | ESP32-S3 Pin |
| :--- | :--- |
| `CLK` | `GPIO11` |
| `MOSI` | `GPIO12` |
| `CS` | `GPIO40` |
| `DC` | `GPIO5` |
| `RESET` | `GPIO41` |

### Prerequisites & Fonts

1. **Google Fonts** (automatically downloaded during compilation):
   * `Roboto` (`13 px`, `16 px`, `20 px`)
   * `Roboto@700` (`42 px`, `56 px`)
2. **Local Icon Font**:
   * Place `materialdesignicons-webfont.ttf` inside the `fonts/` folder of your ESPHome directory (`/config/esphome/fonts/materialdesignicons-webfont.ttf`).

### Required ESPHome Entities & Globals

#### Local & System Sensors
* `wifi` (`id: wifi_component`), `wifi_signal` (`id: wifi_signal_db`), `wifi_info` (`id: wifi_ssid_sensor`, `id: wifi_ip`)
* `time` (`id: esptime`)
* `switch` (`id: mute_switch`) — Microphone mute state
* `sensor`:
  * `temp_sensor` / `hum_sensor` — Indoor temperature and humidity
  * `bat_level` / `bat_voltage` — Battery percentage and voltage

#### Home Assistant Sensors (Outdoor Weather)
* `ha_weather_temperature` — Outdoor temperature (`°C`)
* `ha_weather_humidity` — Outdoor humidity (`%`)
* `ha_weather_wind` — Wind speed (`km/h`)
* `ha_weather_next` (`text_sensor`) — Upcoming weather condition

#### Fallback Global Variables (`globals:`)
Automatically used on boot before sensors report valid states:
* `gtemp`, `ghum`
* `gweather` (index `0`–`9`), `gweather_temp`, `gweather_hum`, `gweather_wind`, `gweather_wind_dir` (index `0`–`7`), `gweather_next`
* `gbat`, `gvolt`

### Installation

1. Copy `materialdesignicons-webfont.ttf` into `esphome/fonts/`.
2. Add the `spi:`, `font:`, `color:`, and `display:` sections to your device's YAML configuration.
3. Compile and flash:
   ```bash
   esphome run your-device.yaml
   ```
