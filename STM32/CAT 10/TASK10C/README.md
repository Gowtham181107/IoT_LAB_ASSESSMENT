EXPERIMENT DETAILS

EXP: CONTINUOUS ASSESSMENT 10C
DATE: 05/10/2026
NAME OF EXP: Connect Wemos ESP8266 to Wi-Fi and Retrieve IP Address
HARDWARE SETUP / PLATFORM: STM32 Development Board (Blue Pill) + Wemos ESP8266 + USB-to-UART Adapter + STM32CubeMX + Keil MDK (HAL Libraries) + PuTTY

TASK 10C: Connect Wemos ESP8266 to Wi-Fi and Retrieve IP Address

1. Circuit Diagram
The experiment establishes dual-UART communication where the STM32 microcontroller interfaces with the Wemos ESP8266 module via USART1 to send AT commands, and forwards all module responses to a PC serial terminal (PuTTY) via USART2 using a USB-to-UART adapter.

Development Board: STM32 Development Board (Blue Pill STM32F103C8T6)
Microcontroller: STM32 (ARM Cortex-M3)
Wi-Fi Module: Wemos D1 Mini (ESP8266)
UART Peripherals: 
  - USART1 (Baud: 115200): Connected to ESP8266 (PA9 TX -> ESP RX, PA10 RX <- ESP TX)
  - USART2 (Baud: 115200): Connected to PC terminal via USB-to-UART adapter (PA2 TX -> Adapter RX, PA3 RX <- Adapter TX)
Power Supply: Powered via external stable 3.3V supply for ESP8266 and USB/ST-Link for STM32.
Ground: Common ground (GND) shared between STM32, ESP8266, and USB-to-UART adapter.

Circuit Diagram
![Circuit Diagram](ESP8266%20Wi-Fi%20Wiring%20and%20IP%20Diagram.png)
ESP8266 Wi-Fi Wiring and IP Diagram.png is a reference schematic showing the STM32 Blue Pill interfaced with the Wemos ESP8266 and USB-to-UART adapter.

2. Core Logic / Pseudocode / Code Base

Working Principle:
The firmware initializes the STM32 system clocks, GPIOs, USART1, and USART2. It communicates with the Wemos ESP8266 using standard AT command sets over USART1. The initialization sequence transmits "AT" to verify module responsiveness, sets the Wi-Fi mode to Station mode (`AT+CWMODE=1`), connects to the wireless router using SSID and password credentials (`AT+CWJAP`), and requests the assigned network IP address (`AT+CIFSR`). All responses returned from the ESP8266 are transparently echoed and forwarded to the PC terminal via USART2 for monitoring in PuTTY.

Pseudocode:
Initialize the STM32 HAL library and system clock.
Configure GPIO pins and initialize USART1 (115200 baud) for ESP8266 communication.
Initialize USART2 (115200 baud) for PC serial terminal forwarding.
Enter the main execution block:
  - Transmit "AT" command via USART1 to check ESP8266 readiness.
  - Receive response from ESP8266 and forward it to PC via USART2.
  - Transmit "AT+CWMODE=1" to configure Station mode.
  - Receive response and forward to PC via USART2.
  - Transmit Wi-Fi join command (`AT+CWJAP="SSID","PASSWORD"`) to associate with the router.
  - Wait for connection confirmation and forward response to PC via USART2.
  - Transmit "AT+CIFSR" to query the local IP address.
  - Capture IP details and forward output to PC via USART2.
  - Enter a permanent delay loop.

Code Base:
The program is written in Embedded C utilizing STM32CubeMX for hardware configuration and Keil MDK for compilation.
`MX_USART1_UART_Init()` and `MX_USART2_UART_Init()` configure the dual serial interfaces.
`HAL_UART_Transmit()` and `HAL_UART_Receive()` handle string and command data exchange between the MCU, Wi-Fi module, and host terminal.

3. Inference
The STM32 microcontroller successfully communicated with the Wemos ESP8266 Wi-Fi module using AT commands over USART1 and relayed output to the PC via USART2. The module correctly interpreted station mode configuration, associated with the wireless access point, and retrieved its dynamic IP address. Proper logic level matching (3.3V) and a dedicated, stable power supply for the ESP8266 were essential for reliable wireless operation and error-free serial logging.