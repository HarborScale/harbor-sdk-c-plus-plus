
# HarborScaleSDK for Arduino (ESP32 / ESP8266)

![Arduino Library](https://img.shields.io/badge/Arduino-Library-00979D.svg)
![Platform](https://img.shields.io/badge/platform-ESP32%20|%20ESP8266-orange.svg)
![License](https://img.shields.io/github/license/harborscale/harbor-sdk-c-plus-plus.svg)

![Stars](https://img.shields.io/github/stars/harborscale/harbor-sdk-c-plus-plus.svg?style=social)

## The Easiest Cloud Dashboard for ESP32 & ESP8266 🚀

**Stop coding HTML web servers for your ESP32.**

The **Harbor Scale SDK** lets you send sensor data from your Arduino/ESP devices to a fully managed Grafana dashboard in **3 lines of code**.

* **⚡ Instant Visualization:** Data appears on your hosted Grafana dashboard in milliseconds.
* **💾 Infinite Storage:** We handle the time-series database. No SD cards required.
* **🔌 Zero Config:** No Docker, no port forwarding, no complex certificate management.

[**👉 Get your Free API Key at HarborScale.com**](https://www.HarborScale.com)

---

## Installation

1.  Open the Arduino IDE.
2.  Go to `Sketch` > `Include Library` > `Manage Libraries...`.
3.  Search for **"HarborScaleSDK"**.
4.  Click **Install**.

---

## 60-Second Quickstart

Here is how to send your first temperature reading.

```cpp
#include <WiFi.h>
#include "HarborClient.h"

// 1. Setup your Wifi & Harbor Credentials
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASS";
const char* harborEndpoint = "https://harobrscale.com/api/v2/ingest/YOUR_HARBOR_ID";
const char* harborApiKey = "sk_live_...";

HarborClient harbor(harborEndpoint, harborApiKey);

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) delay(500);
}

void loop() {
  // 2. Define your data
  GeneralReading reading;
  reading.ship_id = "esp32-sensor-01"; // Your Device Name
  reading.cargo_id = "temperature";    // Your Metric Name
  reading.value = 24.5;                // Your Sensor Value
  // Note: 'time' is optional. If omitted, the server uses reception time.

  // 3. Send it
  int status = harbor.send(reading);
  
  if (status == 200) {
    Serial.println("Data sent! Check your Grafana dashboard.");
  }

  delay(10000); // Send every 10 seconds
}
````

-----

## Advanced Usage

### Sending Batches (Save Battery & Data)

Sending one HTTP request per reading is inefficient. Use `sendBatch` to send multiple metrics at once.

```cpp
void loop() {
  GeneralReading readings[2];

  // Temperature
  readings[0].ship_id = "esp32-sensor-01";
  readings[0].cargo_id = "temperature";
  readings[0].value = 24.5;

  // Humidity
  readings[1].ship_id = "esp32-sensor-01";
  readings[1].cargo_id = "humidity";
  readings[1].value = 60.2;

  // Send both in one request
  harbor.sendBatch(readings, 2);
  
  delay(60000); // Sleep for 1 minute
}
```

### Logging GPS Data

Harbor Scale automatically handles GPS data for map visualizations. Simply send `latitude` and `longitude` as separate metrics.

```cpp
  readings[0].cargo_id = "latitude";
  readings[0].value = 40.7128;
  
  readings[1].cargo_id = "longitude";
  readings[1].value = -74.0060;
  
  harbor.sendBatch(readings, 2);
```

-----

## Features

  * ✅ **JSON Handling**: Automatically handles serialization (uses ArduinoJson internally).
  * ✅ **Auto-Retry**: Built-in exponential backoff for when Wi-Fi is flaky.
  * ✅ **Efficient**: Minimal memory footprint, optimized for embedded devices.
  * ✅ **Secure**: Full HTTPS support.

-----

## Need Help?

  * 📚 **Documentation:** [docs.harborscale.com](https://docs.harborscale.com)
  * 💬 **Support:** [support@harborscale.com](mailto:support@harborscale.com)

<!-- end list -->

