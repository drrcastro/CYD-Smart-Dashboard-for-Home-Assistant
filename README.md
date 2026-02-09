📱 CYD Smart Dashboard for Home Assistant
A clean, fast, and fully customizable touch dashboard for the ESP32-2432S028 (widely known as the "Cheap Yellow Display" or CYD).

Built with ESPHome and LVGL, this project allows you to monitor 6 key Home Assistant sensors and trigger automations/scripts by tapping on them. It features automatic brightness control and a header-based configuration system, making it easy to adapt to your own home without touching the complex code.

(Replace this with a photo of your dashboard)

✨ Features
6 Customizable Slots: Monitor entities like Solar, Grid, Battery, Water Temp, Outdoor Weather, etc.

Touch Actions: Tap any sensor block to trigger a specific Home Assistant Script or Automation.

Dynamic Colors: Values change color based on thresholds (e.g., Solar turns gold when generating, Battery turns red when low).

Auto-Brightness: Uses the onboard LDR sensor with a custom polynomial curve to smoothly adjust backlight based on ambient light.

Easy Configuration: All settings (Entities, Icons, Labels, Actions) are defined at the top of the YAML file. No need to dig into the LVGL code.

Zero Ghost Touches: optimized touch logic using on_press for resistive screens.

🛠️ Hardware
Board: ESP32-2432S028R (The standard Resistive Touch version).

Display: 2.8" ILI9341 (320x240).

Touch: XPT2046.

Sensor: Onboard LDR (Light Dependent Resistor) for auto-brightness.

🚀 Installation
1. Prerequisites
Home Assistant with the ESPHome add-on installed.

A CYD board connected via USB.

2. Copy the Code
Create a new device in ESPHome.

Copy the content of cyd_dashboard.yaml from this repository.

Paste it into your new device's configuration.

3. Configure Secrets
Ensure your secrets.yaml in ESPHome has your WiFi credentials:

YAML
wifi_ssid: "YOUR_WIFI_NAME"
wifi_password: "YOUR_WIFI_PASSWORD"
⚙️ Configuration (The Easy Part)
You don't need to edit the complex C++/Lambda code. Just look for the substitutions: section at the top of the file.

Example: Configuring Sensor 1
YAML
substitutions:
  # ...
  s1_entity: "sensor.solar_production"      # Your HA Entity ID
  s1_icon: "\uec0f"                         # Material Icon Code (see below)
  s1_label: "Solar Power"                   # Text Label
  s1_unit: "W"                              # Unit (use %% for percentage)
  s1_tap_action: "script.toggle_inverter"   # Script to run on tap
🎨 How to change Icons
This project uses Material Design Icons.

Go to Pictogrammers / Material Design Icons.

Find an icon you like.

Copy its Hex Code (e.g., F061).

Prefix it with \u in the YAML config (e.g., \uf061).

⚠️ Important: Enabling Actions
By default, Home Assistant blocks ESPHome devices from calling services (running scripts) for security reasons.

If your buttons don't work, do this:

Go to Home Assistant > Settings > Devices & Services.

Click on ESPHome.

Find your CYD device and click Configure.

Enable the option: "Allow the device to perform Home Assistant service calls".

Restart the CYD (Unplug and plug back in).

💡 Auto-Brightness Logic
The code includes a calibrated curve for the CYD's LDR. Note that on many CYD boards, the LDR logic is inverted (Lower Voltage = More Light).

The current configuration is tuned for:

0.14V (Bright Light): 100% Backlight

1.40V (Dark Room): 5% Backlight

🤝 Contributing
Feel free to fork this project and submit pull requests if you improve the UI or add new features like multiple pages!

📄 License
This project is open-source. Use it, modify it, and share it!
