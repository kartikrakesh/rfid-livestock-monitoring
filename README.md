# RFID Livestock Monitoring System

An STM32-based system that identifies animals by their RFID tag, takes a temperature reading with a BME280 sensor, and sends both to a browser dashboard over USB. The dashboard logs every scan with its date and time and can export the full history as a CSV file.

<!-- Add a screenshot of the dashboard here, e.g. ![Dashboard](docs/dashboard.png) -->

## Features

- Identifies individual animals by RFID tag ID
- Measures temperature with a Bosch BME280 sensor
- Sends readings to the PC through the STM32's native USB port, with no separate USB-to-serial adapter
- Web dashboard with a live ID and temperature display, a timestamped acquisition history, and one-click CSV export
- The dashboard is a single HTML file, so it needs no server or installation

## Hardware

| Component        | Details                          |
|------------------|----------------------------------|
| Microcontroller  | STM32F103C8T6 (Cortex-M3, 72 MHz) |
| RFID reader      | MFRC522 (RC522), 13.56 MHz, SPI  |
| Sensor           | BME280                           |
| Programmer       | ST-LINK                          |
| PC connection    | USB (STM32 USB device)           |

### Wiring

All pin assignments are defined in `RFID.ioc`. Open it in STM32CubeMX or STM32CubeIDE to see the full pinout.

<!--
| Module      | Module pin | STM32 pin |
|-------------|------------|-----------|
| RC522       |            |           |
| BME280      |            |           |
-->

## Software

- STM32CubeIDE
- STM32CubeMX, for peripheral configuration (built into CubeIDE)
- STM32F1 HAL drivers
- ST USB Device Library
- Chrome, Edge or Brave, for the dashboard

## Repository structure

```
├── Core/                              Application code: main.c, peripheral setup, interrupts, startup
├── Drivers/                           STM32F1 HAL and CMSIS drivers
├── Middlewares/                       ST USB Device Library
├── USB_DEVICE/                        USB device configuration and application layer
├── RFID.ioc                           STM32CubeMX project configuration
├── STM32F103C8TX_FLASH.ld             Linker script
├── RFID-BME280 Debug.launch           Debug/flash configuration for STM32CubeIDE
├── RFID-BME280 Debug.cfg              Debug probe configuration
└── Livestock_Monitoring_Dashboard.html   Web dashboard
```

## Getting started

### 1. Build and flash the firmware

1. Clone the repository:
   ```
   git clone https://github.com/kartikrakesh/rfid-livestock-monitoring.git
   ```
2. In STM32CubeIDE, go to **File → Import → General → Existing Projects into Workspace** and select the cloned folder.
3. Build the project with **Project → Build Project** or the hammer icon. Build output goes to `Debug/`, which is not tracked in Git.
4. Connect the ST-LINK to the board and run the **RFID-BME280 Debug** configuration to flash it.

To change peripheral settings, open `RFID.ioc`, make your changes and regenerate the code. Anything written between the `USER CODE BEGIN` and `USER CODE END` comments is kept when the code is regenerated.

### 2. Run the dashboard

1. Connect the board to the PC with a USB cable.
2. Open `Livestock_Monitoring_Dashboard.html` in Chrome, Edge or Brave.
3. Click **Start Scanning**.
4. Hold an RFID tag near the reader. The tag ID and temperature appear on screen and are added to the **Acquisition History** table with the date and time.
5. Click **Export CSV File** to download the history.

## How it works

1. The RFID reader detects a tag, and the STM32 reads its unique ID.
2. The STM32 takes a temperature reading from the BME280.
3. The ID and temperature are sent to the PC over USB.
4. The dashboard shows the reading and adds it, with the date and time, to the history table, which can be exported as CSV.

## Author

Kartik Rakesh ([@kartikrakesh](https://github.com/kartikrakesh))
