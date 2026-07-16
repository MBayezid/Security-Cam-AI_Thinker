# 📖 Project Documentation: Smart_Security_Camera-AI_Thinker

**Repository:** [https://github.com/MBayezid/Smart_Camera_System-AI_Thinker](https://github.com/MBayezid/Smart_Camera_System-AI_Thinker)

## 📌 Overview
**Smart_Camera_System-AI_Thinker** is an open-source, low-cost IoT surveillance project based on the ESP32-CAM (AI-Thinker) module. It functions as a standalone wireless smart camera that provides live MJPEG video streaming and on-demand snapshot capture via a lightweight, embedded web server. 

---

## 🎯 Target Audience
*   **Users:** DIY home monitoring enthusiasts, hobbyists, and makers looking for an affordable, customizable alternative to commercial security cameras.
*   **Developers:** IoT engineers and students looking for a foundational ESP32-CAM web server implementation to build upon (e.g., adding cloud integration, AI, or motion detection).

---

## ✨ Key Features (v1.0.0)
*   **Live Video Streaming:** HTTP-based MJPEG stream accessible via standard web browsers (Target: 10-15 FPS on SVGA).
*   **Snapshot Capture:** On-demand high-resolution JPEG image capture via a web interface button or direct API endpoint.
*   **Lightweight Web Server:** Custom HTML interface hosted directly on the ESP32.
*   **Hardware Optimized:** Automatically detects PSRAM to adjust frame buffer and resolution (SVGA vs. VGA) for optimal performance.
*   **Auto-Reconnect Logic:** Basic timeout and restart protocols if Wi-Fi fails to connect on boot.

---

## 🛠️ Hardware Requirements
| Component | Specification |
| :--- | :--- |
| **Microcontroller** | ESP32-CAM Module (AI-Thinker pinout) |
| **Camera Sensor** | OV2640 (2MP, included with most ESP32-CAM boards) |
| **Power Supply** | 5V DC via USB or dedicated 5V/GND pins |
| **Storage (Optional)** | microSD card (FAT32, <= 32GB recommended) |
| **Flashing Tool** | FTDI Programmer (USB-to-TTL) required for initial upload |

---

## 💻 Developer Setup & Configuration

### 1. Prerequisites
*   [Visual Studio Code](https://code.visualstudio.com/)
*   [PlatformIO IDE Extension](https://platformio.org/)
*   Git

### 2. Installation
Clone the repository and open it in VS Code:
```bash
git clone https://github.com/MBayezid/Smart_Camera_System-AI_Thinker.git
cd Smart_Camera_System-AI_Thinker
```

### 3. Configuration
Before compiling, you must configure your Wi-Fi credentials. You have two options based on `main.cpp`:
*   **Option A (Recommended):** Create a `secrets.h` file in the `src/` or `include/` directory and define your credentials:
    ```cpp
    #define WIFI_SSID "YOUR_WIFI_SSID"
    #define WIFI_PASS "YOUR_WIFI_PASS"
    ```
*   **Option B:** Uncomment the hardcoded credential lines directly inside `main.cpp`:
    ```cpp
    const char* WIFI_SSID = "YOUR_WIFI_SSID";
    const char* WIFI_PASS = "YOUR_WIFI_PASS";
    ```

### 4. Build & Upload
1. Connect the ESP32-CAM to your PC via an FTDI programmer. *(Note: GPIO 0 must be connected to GND during the upload process to enter flash mode).*
2. Select the `esp32cam` environment in PlatformIO.
3. Click **Upload**.
4. Once uploaded, disconnect GPIO 0 from GND and press the **RESET** button on the ESP32-CAM.

---

## 📱 User Guide (How to Use)

1. **Power On:** Supply 5V to the ESP32-CAM.
2. **Find IP Address:** Open the Serial Monitor in PlatformIO (Baud rate: `115200`). Wait for the device to connect to Wi-Fi and print its local IP address (e.g., `192.168.1.50`).
3. **Access Interface:** Open a web browser on a device connected to the *same Wi-Fi network* and navigate to `http://<ESP32-IP-ADDRESS>/`.
4. **Web Endpoints:**
   * `/` : The main HTML dashboard containing the live stream view and a snapshot button.
   * `/stream` : Direct access to the raw MJPEG stream (useful for embedding in other apps like VLC, OctoPrint, or custom dashboards).
   * `/capture` : Triggers and downloads a single high-resolution JPEG snapshot.

---

## 🗺️ Roadmap & Future Enhancements (Phase 3)
Based on the Product Requirements Document (PRD), future iterations aim to include:
*   [ ] **SD Card Integration:** Local storage for saving snapshots or recording short video clips.
*   [ ] **Motion Detection:** Basic algorithmic detection to trigger snapshots or HTTP API alerts.
*   [ ] **External API Integration:** Push notifications or cloud uploads via HTTP/HTTPS webhooks.
*   [ ] **OTA Updates:** Over-The-Air firmware updates to eliminate the need for physical FTDI connections.
*   [ ] **Dynamic Wi-Fi Setup:** Captive portal or API-driven credential management (eliminating hardcoded passwords).
*   [ ] **Robust Connection Handling:** Auto-reconnection logic if the Wi-Fi signal drops during operation.

---

## ⚠️ Developer Notes (Code Review Observations)
If you are pulling the current `main.cpp` directly from the repository, please note that the file contains several typographical/OCR errors (likely from copy-pasting) that will cause compilation failures in C++. **Ensure you clean up the syntax before building:**
*   Fix broken variable names/macros (e.g., `Y5 _GPIO_NUM` $\rightarrow$ `Y5_GPIO_NUM`, `HREF_G PIO_NUM` $\rightarrow$ `HREF_GPIO_NUM`).
*   Fix broken keywords (e.g., `esp _err_t` $\rightarrow$ `esp_err_t`, `WL_CON NECTED` $\rightarrow$ `WL_CONNECTED`).
*   Fix broken functions (e.g., `esp_camera_fb_r eturn` $\rightarrow$ `esp_camera_fb_return`, `hand leRoot` $\rightarrow$ `handleRoot`).
*   Remove spaces in HTTP headers (e.g., `C ontent-Type` $\rightarrow$ `Content-Type`).
*   Ensure `#include "secrets.h "` does not have a trailing space inside the quotes.
