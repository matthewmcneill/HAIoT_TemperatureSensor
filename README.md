# HAIoT_TemperatureSensor

This Arduino project demonstrates how to integrate a temperature sensor with Home Assistant using MQTT for seamless monitoring and control within your smart home ecosystem.

## Overview

The code in this repository provides the following functionalities:

* **Temperature Sensing:** Reads temperature data from a DS18B20 sensor connected to the Arduino.
* **MQTT Communication:** Utilizes the `PubSubClient` library to establish a connection to an MQTT broker and publish temperature readings.
* **Home Assistant Integration:** Leverages the `ArduinoHA` library to seamlessly integrate the temperature sensor as a sensor entity in Home Assistant.
* **Configuration:** Stores Wi-Fi and MQTT configuration details in the Arduino's persistent storage (EEPROM or Preferences).
* **OTA Updates (Work in Progress):** The code includes provisions for Over-The-Air (OTA) firmware updates, although this feature might still be under development.

## Hardware Requirements

* **Arduino Board:** This code is designed for the Arduino Nano 33 IoT and the Nano ESP32 S3.
* **DS18B20 Temperature Sensor:** You'll need a DS18B20 temperature sensor (or a compatible module) connected to your Arduino. Refer to the comments in the code or the sensor's datasheet for wiring instructions.
* **Wi-Fi Connection:** Ensure your Arduino board has access to a Wi-Fi network.
* **MQTT Broker:** You'll need a running MQTT broker (e.g., Mosquitto, HiveMQ) on your network or a cloud-based MQTT service.

## Software Requirements

* **Arduino IDE:** Install the latest version of the Arduino IDE.
* **Libraries:**
   * Install the following libraries through the Arduino Library Manager:
     * `ArduinoHA`
     * `PubSubClient`
     * `WiFiNINA` (for Nano 33 IoT) or `WiFi` (for Nano ESP32 S3)
     * `OneWire`
     * `DallasTemperature`

## How it Works

1. **Configuration:**
   * The code reads Wi-Fi and MQTT configuration details from the `config` object (defined in `sys_config.h`).
   * This can be interactively configured from the serial port in the IDE if the Ardunio is connected.
   * These settings are stored in the Arduino's persistent storage using `Preferences` (for Nano 33 IoT and Nano ESP32 S3).

2. **Wi-Fi Connection:**
   * The `setupWifi()` function attempts to connect to the configured Wi-Fi network.
   * It includes a timeout mechanism and error handling to ensure a robust connection process.

3. **MQTT Connection:**
   * Once the Wi-Fi connection is established, the `setupHA()` function initializes the MQTT client and connects to the MQTT broker using the provided credentials.
   * It also sets up the device's availability and last will topics for Home Assistant integration.

4. **Temperature Sensor Setup:**
   * The `setupTempSensors()` function (defined in `sensor_temperature.h`) configures the DS18B20 temperature sensor and registers it with Home Assistant using the `ArduinoHA` library.

5. **Temperature Updates:**
   * The `updateTemperatureThread()` function (also in `sensor_temperature.h`) periodically reads the temperature from the sensor and publishes it to the appropriate MQTT topic for Home Assistant to consume.
   * The update interval is controlled by a `Thread` object managed by a `ThreadController`.

6. **Main Loop:**
   * The `loop()` function in the main sketch calls `ha.loop()` to handle MQTT communication and trigger any scheduled events (temperature updates).

## Getting Started

1. **Configure `sys_config.h`:** No need to fill in your Wi-Fi credentials, MQTT broker details, or other configuration settings in the `sys_config.h` file - it can be set up interactively from the serial port.
2. **Connect the Sensor:** Wire the DS18B20 temperature sensor to your Arduino board according to the instructions in the code or the sensor's datasheet.
3. **Upload the Sketch:** Compile and upload the sketch to your Arduino.
4. **Monitor in Home Assistant:** Once the Arduino connects to Wi-Fi and MQTT, you should see the temperature sensor entity appear in Home Assistant. You can then use it in automations, dashboards, or other parts of your smart home setup.

**Disclaimer**

This project is provided as-is. Use it at your own risk. The author is not responsible for any damages or issues that might arise from using this code. Please ensure you understand the code and its implications before deploying it in a production environment.
