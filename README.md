# 📱 CYD Smart Dashboard for Home Assistant

A clean, fast, and fully customizable touch dashboard for the **ESP32-2432S028** (widely known as the "Cheap Yellow Display" or CYD).

Built with **ESPHome** and **LVGL**, this project allows you to monitor 6 key Home Assistant sensors and trigger automations/scripts by tapping on them. It features automatic brightness control and a header-based configuration system, making it easy to adapt to your own home without touching the complex code.

![Dashboard Preview](/preview.jpeg)


## ✨ Features

* **6 Customizable Slots:** Monitor entities like Solar, Grid, Battery, Water Temp, Outdoor Weather, etc.
* **Touch Actions:** Tap any sensor block to trigger a specific Home Assistant Script or Automation.
* **Dynamic Colors:** Values change color based on thresholds (e.g., Solar turns gold when generating, Battery turns red when low).
* **Auto-Brightness:** Uses the onboard LDR sensor with a custom polynomial curve to smoothly adjust backlight based on ambient light.
* **Easy Configuration:** All settings (Entities, Icons, Labels, Actions) are defined at the top of the YAML file.
* **Zero Ghost Touches:** Optimized touch logic using `on_press` for resistive screens.

## 🛠️ Hardware

* **Board:** ESP32-2432S028.
* **Display:** 2.8" ILI9341.
* **Touch:** XPT2046.
* **Sensor:** Onboard LDR (Light Dependent Resistor) on GPIO34 for auto-brightness.
* **Case:** 3D Printed Case ([I used this one](https://www.thingiverse.com/thing:6653040)).

## 🚀 Installation

### 1. Prerequisites
* Home Assistant with the **ESPHome** add-on installed.
* A CYD board connected via USB.

### 2. Copy the Code
1.  Create a new device in ESPHome.
2.  Copy the content of `cyd_dashboard.yaml` from this repository.
3.  Paste it into your new device's configuration.
4. Change the inicial config to match yours

## ⚙️ Configuration (The Easy Part)

You don't need to edit the complex C++/Lambda code. Just look for the **`substitutions:`** section at the top of the file.

### Example: Configuring Sensor 1
```yaml
substitutions:
  # ...
  s1_entity: "sensor.solar_production"      # Your HA Entity ID
  s1_icon: "\uec0f"                         # Material Icon Code
  s1_label: "Solar Power"                   # Text Label
  s1_unit: "W"                              # Unit (use %% for percentage)
  s1_tap_action: "script.toggle_inverter"   # Script to run on tap
```

### 🎨 How to change Icons
This project uses **Material Design Icons**.
1.  Go to [Pictogrammers / Material Design Icons](https://pictogrammers.com/library/mdi/).
2.  Find an icon you like.
3.  Copy its **Hex Code** (e.g., `F061`).
4.  Prefix it with `\u` in the YAML config (e.g., `\uf061`).
5.  **Important:** You must also add this code to the `glyphs:` list in the `font:` section of the YAML file for it to render.

### 🔡 Custom Fonts
The dashboard uses Google's **Roboto** font and **Material Design Icons**. If you need to add special characters (like `€`, `£`) or new icons, make sure their codes are included in the `glyphs:` string under the `font:` section in your YAML file, otherwise they will show up as empty boxes.

## ⚠️ Important: Enabling Actions

By default, Home Assistant blocks ESPHome devices from calling services (running scripts) for security reasons. 

**If your buttons don't work, do this:**
1.  Go to Home Assistant > **Settings** > **Devices & Services**.
2.  Click on **ESPHome**.
3.  Find your CYD device and click **Configure**.
4.  Enable the option: **"Allow the device to perform Home Assistant service calls"**.
5.  **Restart the CYD** (Unplug and plug back in).

## 💡 Auto-Brightness Logic

The code includes a calibrated polynomial curve for the CYD's LDR. Note that on many CYD boards, the LDR logic is inverted (Lower Voltage = More Light).

The current configuration is tuned for:
* **0.14V (Bright Light):** 100% Backlight
* **1.40V (Dark Room):** 5% Backlight

## 🤝 Contributing

Feel free to fork this project and submit pull requests if you improve the UI or add new features like multiple pages!

## 📄 License

This project is open-source. Use it, modify it, and share it!
