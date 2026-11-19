# ESP32-S3-RLCD-4.2 — ESPHome Configuration

A complete ESPHome YAML configuration for the **Waveshare ESP32-S3-RLCD-4.2** development board (4.2" 400×300 Reflective LCD display).

This setup turns the board into a low-power indoor/outdoor weather station and a Home Assistant Voice Assistant node.

![Screen](docs/20261119_esp32-s3-rlcd-4.2.jpg)

---

## ✨ Features

* **4.2" Reflective LCD (`st7305`)**: Uses the native ESPHome `st7305` display driver (ESPHome **2026.11.0+**).
* **Indoor Climate**: Reads local temperature and humidity via the onboard **SHTC3** I2C sensor.
* **Outdoor Weather**: Fetches outdoor temperature, humidity, and weather condition icons directly from a Home Assistant `weather` entity.
* **Network & Status Header/Footer**: Displays Wi-Fi connection status, microphone mute indicator, connected SSID, IP address, and RTC-backed date/time (`pcf85063`).
* **Battery Monitoring & Deep Sleep**: Tracks battery voltage and percentage with a visual progress bar, plus configurable deep sleep cycles controlled from Home Assistant.
* **Home Assistant Voice Assistant**: Supports onboard wake word detection (`micro_wake_word` with *"Hey Jarvis"*), ES8311 DAC audio playback, ES7210 ADC microphone input, and hardware buttons.

---

## 📋 Requirements

* **Hardware**: [Waveshare ESP32-S3-RLCD-4.2](https://waveshare.com)
* **ESPHome**: Version **2026.11.0** or newer (required for native `st7305` support).
* **Fonts**: Place `materialdesignicons-webfont.ttf` inside your ESPHome `fonts/` directory.

---

## ⚙️ Configuration & Customization

Before compiling and flashing `esp32s3rlcd.yaml`, update the following parameters to match your environment:

### 1. `secrets.yaml`
Ensure your ESPHome `secrets.yaml` file contains your Wi-Fi credentials:
```yaml
wifi_ssid: "Your_WiFi_SSID"
wifi_password: "Your_WiFi_Password"
```

### 2. Static IP Address
Update or remove the `manual_ip` section under `wifi:` to match your local network:
```yaml
wifi:
  manual_ip:
    static_ip: 192.168.0.102
    gateway: 192.168.0.254
    subnet: 255.255.255.0
```

### 3. Home Assistant Entities
* **Weather Entity**: Change the `.variables` anchor at the top of the YAML file to your Home Assistant weather entity:
  ```yaml
  .variables:
    - &weather_entity "weather.forecast_home"
  ```
* **OTA Mode Helper**: Create an `input_boolean` helper in Home Assistant to prevent deep sleep during over-the-air updates, and update its entity ID under `binary_sensor:`:
  ```yaml
  - platform: homeassistant
    name: "OTA Mode"
    id: ota_mode
    entity_id: input_boolean.ota_mode
  ```

---

## 🔘 Hardware Controls

| Pin | Function | Action |
| :--- | :--- | :--- |
| **GPIO0** (`Boot`) | Push-to-Talk | Hold to start Voice Assistant, release to stop |
| **GPIO18** (`Key`) | Mute / Wakeup | Toggles microphone mute state; also wakes the ESP32 from deep sleep |

---

## 🔋 Deep Sleep Behavior

* By default, **Prevent Sleep** is enabled (`RESTORE_DEFAULT_ON`) so the Voice Assistant remains responsive.
* Turn off **Prevent Sleep** in Home Assistant to enable battery-saving deep sleep cycles (duration configurable via the **Sleep Duration** number entity in hours).
* Enabling **OTA Mode** in Home Assistant automatically blocks deep sleep on boot so you can push firmware updates reliably.

---

## 📄 License

This project is open-source and available under the MIT License.
