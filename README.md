# ESP32 Bluetooth serial HCI adapter

This project turns an ESP32 board into a Bluetooth serial HCI adapter.

## Preparing

- Connect ESP32`s RTS and CTS to the corresponding UART adapter pins;
- Flash the provided firmware to your ESP32 board.

Flashing command line example.
python.exe .platformio\packages\tool-esptoolpy\esptool.py --chip esp32 --port "COM1" --baud 460800 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size 4MB 0x1000 bootloader.bin 0x8000 partitions.bin 0x10000 firmware.bin

RTS/CTS connections are mandatory.
Example of RTS/CTS signal connection for a board with a CH340 USB-UART controller
![RTS/CTS connection to CH340](/images/esp32_rtscts.jpg)

## Usage

On Linux, use the following command:
hciattach -s 921600 /dev/ttyUSB0 any 921600 flow
Then the device should become available to the system as a Bluetooth controller.

## Known issues

The ESP32 firmware may crash if LOCAL_NAME is not set. Workaround: set local name by using WRITE_LOCAL_NAME command.

The project is based on the UART HCI Controller example from the Espressif IoT Development Framework (ESP-IDF).
The UART port was changed from 1 to 0 (USB-UART port), and the incorrect pin assignments were fixed.