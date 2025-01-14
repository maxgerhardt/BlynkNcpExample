# Building a new device using Blynk.NCP

![Blynk.NCP connection](./Images/BlynkNCP.png)

## Components Used in This Project

You will need:

- Raspberry Pi Pico
- One of the [supported connectivity modules][supported_modules]
- An adapter/breakout board for your connectivity module (in some cases)
- Breadboard and some jumper wires

## 1. Flash `Blynk.NCP` firmware to your connectivity module

Please follow the official firmware flashing guide. This is usually provided by the module vendor.  
Blynk.NCP is shipped as a combined firmware, so you only need to flash a single file (flash at address `0`).  
Select the [firmware file, corresponding to your module type][supported_modules].

To verify that `Blynk.NCP` is flashed correctly and running, check the UART output (115200 8N1) of the module, right after the power-up:

```log
<...bootloader output...>
[rpc port] Blynk.NCP started
```

## 2. Assemble the board

Connect Blynk.NCP to the Primary MCU of your choice (i.e. `STM32`, `ATmega`, `RP2040`, `ATSAMD`, `K210`, ...) using UART.  
For example, here's how to connect the XBee form factor modules (`Macchina SuperB`, `DFRobot WiFi Bee`) to the `Raspberry Pi Pico` board:

![Blynk.NCP breadboard](./Images/PiPico-XBee-BlynkNCP.png)

## 3. Use the Blynk library to communicate with the NCP

The official [`Blynk library`](https://github.com/blynkkk/blynk-library) provides everything needed to work with **Blynk.NCP**.

1. Use [this example](https://github.com/blynkkk/BlynkNcpExample)
2. Open `src/main.cpp` and fill in [information from your Blynk Template](https://bit.ly/BlynkInject):

    ```cpp
    #define BLYNK_TEMPLATE_ID           "TMPxxxxxx"
    #define BLYNK_TEMPLATE_NAME         "MyDevice"
    ```
3. Run the example on your **Primary MCU**:

    ```sh
    pio run -e pico -t upload -t monitor
    ```
   The expected debug output looks like this:

    ```log
    [1345] NCP responding (baud 115200, 2289 us)
    [1349] Blynk.NCP firmware: x.x.x
    [1684] State: Configuration
    ```

> __Note__: Blynk C++ library uses [`BlynkNcpDriver`](https://github.com/blynkkk/BlynkNcpDriver) internally to talk to the NCP.
You can use this low-level, `C99` compatible library with minimal dependencies directly if you prefer. See [BlynkNcpDriver example](https://github.com/blynkkk/BlynkNcpDriver/blob/main/examples/BlynkNCP_Simple/BlynkNCP_Simple.ino).

## 4. Use the Blynk iOS/Android app to configure your new device

Ensure that the Blynk App is installed on your smartphone and scan this QR code:

<img alt="Add New Device QR" src="./Images/AddNewDeviceQR.png" width="250" />

Alternatively: Open the `Blynk App` -> click `Add New Device` -> select `Find Devices Nearby`

## 5. Next steps

When using `BlynkNcpDriver` directly or integrating `Blynk.NCP` with non-Arduino boards, additional development may be needed to support features like the primary MCU OTA upgrades, Real-Time Clock integration, etc.

Please check out the [BlynkNcpDriver docs](https://github.com/blynkkk/BlynkNcpDriver/tree/main/docs) for further details.

## Disclaimer

> The community edition of Blynk.NCP is available for personal use and evaluation.  
If you're interested in using Blynk.NCP for commercial applications, feel free to [contact Blynk][blynk_sales]. Thank you!

[supported_modules]: https://docs.blynk.io/en/getting-started/supported-boards#connectivity-modules-supported-by-blynk.ncp
[blynk_sales]: https://blynk.io/en/contact-us-business
