# 📱 CYD Smart Dashboard for Home Assistant

A clean, fast, and fully customizable touch dashboard for the **ESP32-2432S028** (widely known as the "Cheap Yellow Display" or CYD).

Built with **ESPHome** and **LVGL**, this project allows you to monitor 6 key Home Assistant sensors and trigger automations/scripts by tapping on them. It features automatic brightness control and a highly optimized "Master Template" configuration system, making it incredibly easy to adapt to your own home without touching the complex underlying code.

![Dashboard Preview](/preview.jpeg)

## 🌟 What's New in v2.1 (Master Template Update)
* **All-in-One Configuration:** The `substitutions` block is now placed at the very top of the file, right below your WiFi/API setup. You never have to scroll through complex C++ or LVGL code.
* **Auto-Compiling Fonts (No More Missing Icons!):** You no longer need to manually add every single icon to the font `glyphs:` list. When you define an icon in the substitutions, the dashboard automatically injects it into the font compiler for you.
* **Exposed Logic:** The C++ `if/else` logic for color thresholds and dynamic icons is now exposed directly in the `substitutions` block, making it effortless to tweak value ranges.

## ✨ Features

* **6 Customizable Slots:** Monitor entities like Solar, Grid, Battery, Water Temp, Outdoor Weather, etc.
* **Touch Actions:** Tap any sensor block to trigger a specific Home Assistant Script or Automation.
* **Dynamic Colors & Icons:** Values change color based on thresholds (e.g., Solar turns gold when generating, Battery changes icon and turns red when low).
* **Auto-Brightness:** Uses the onboard LDR sensor with a custom polynomial curve to smoothly adjust backlight based on ambient light.
* **Zero Ghost Touches:** Optimized touch logic using `on_press` for resistive screens.

## 🛠️ Hardware

* **Board:** ESP32-2432S028.
* **Display:** 2.8" ILI9341.
* **Touch:** XPT2046.
* **Sensor:** Onboard LDR (Light Dependent Resistor) on GPIO34 for auto-brightness.
* **Case:** 3D Printed Case ([I used this one](https://www.thingiverse.com/thing:6653040)) and also made [this one](https://makerworld.com/pt/models/2541785-cyd-2-8-efapel-logus-90-case) if you want to use on EFAPEL switches .

## 🚀 Installation

### 1. Prerequisites
* Home Assistant with the **ESPHome** add-on installed.
* A CYD board connected via USB.

### 2. Copy the Code
1.  Create a new device in ESPHome and let it generate the default WiFi, API, and OTA configuration.
2.  Copy the `substitutions:` block and everything below it from `cyd_dashboard.yaml` in this repository.
3.  Paste it into your new device's configuration, right below the default setup blocks.
4.  Change the `substitutions:` block to match your Home Assistant entities.

## ⚙️ Configuration (The Easy Part)

You don't need to edit the complex code at the bottom. Just look for the **`substitutions:`** section. The variable `x` represents the numeric value coming from the Home Assistant sensor.

### Example: Configuring Sensor 1 (Solar)
```yaml
substitutions:
  # ...
  s1_entity: "sensor.solar_production"      # Your HA Entity ID
  s1_label: "Solar Power"                   # Text Label
  s1_unit: "W"                              # Unit (use %% for percentage)
  s1_tap_action: "script.toggle_inverter"   # Script to run on tap
  s1_icon: "\uec0f"                         # Material Icon Code
  
  s1_icon_logic: |-
    return "${s1_icon}"; 
    
  s1_color_logic: |-
    if (x > 2000) return lv_color_hex(0xFFA500); // High Production (Orange)
    if (x > 0) return lv_color_hex(0xFFD700);    // Normal Production (Yellow)
    return lv_color_hex(0x888888);               // Zero Production (Grey)
```
### 🎨 How to change Icons
This project uses **Material Design Icons**.
1.  Go to [Google Material Icons](https://fonts.google.com/icons).
2.  Find an icon you like.
3.  Look for its **Code point** (e.g., `e1ff`).
4.  Prefix it with `\u` in the YAML config (e.g., `\ue1ff`) and place it in the `sX_icon` variable.
5.  **That's it!** The system will automatically compile the font for you. 

*(Note: If you are building a dynamic sensor that uses multiple alternating icons, like a battery, you must list all of them in a variable like `s5_all_dynamic_icons` so the compiler knows to download them all).*

## ⚠️ Important: Enabling Actions

By default, Home Assistant blocks ESPHome devices from calling services (running scripts) for security reasons. 

**If your buttons don't work, do this:**
1.  Go to Home Assistant > **Settings** > **Devices & Services**.
2.  Click on **ESPHome**.
3.  Find your CYD device and click **Configure**.
4.  Enable the option: **"Allow the device to perform Home Assistant service calls"**.
5.  **Restart the CYD** (Unplug and plug back in).


## 🎯 Touchscreen Calibration

Because these CYD boards are mass-produced, the touchscreens can sometimes be slightly misaligned out of the box. If your screen registers touches slightly above or below where your finger is, you should calibrate it.

1. Open the **ESPHome dashboard** in Home Assistant.
2. Click on **Logs** for your CYD device (choose the wireless option if it's already running).
3. Using a stylus or your finger, carefully press the very edges and corners of the screen (Top-Left, Top-Right, Bottom-Left, Bottom-Right).
4. Watch the live log output. You will see lines like this:
   `[D][xpt2046:085]: Touch pressed at x=250, y=3800`
5. Note down the **lowest** and **highest** values you get for both `x` and `y`.
6. Go back to your YAML code and scroll down to the `touchscreen:` section (around line 240). Update the `calibration:` block with your new values:

```yaml
touchscreen:
  platform: xpt2046
  id: my_touchscreen
  spi_id: touch
  cs_pin: GPIO33
  calibration:
    x_min: 220    # Replace with your lowest X log value
    x_max: 3756   # Replace with your highest X log value
    y_min: 394    # Replace with your lowest Y log value
    y_max: 3749   # Replace with your highest Y log value
  transform:
    swap_xy: true
    mirror_x: false
    mirror_y: false
```
## 💡 Auto-Brightness Logic

The code includes a calibrated polynomial curve for the CYD's LDR. Note that on many CYD boards, the LDR logic is inverted (Lower Voltage = More Light).

The current configuration is tuned for:
* **0.14V (Bright Light):** 100% Backlight
* **1.40V (Dark Room):** 5% Backlight

## 🤝 Contributing

Feel free to fork this project and submit pull requests if you improve the UI or add new features like multiple pages!

## 📄 License

This project is open-source. Use it, modify it, and share it!
