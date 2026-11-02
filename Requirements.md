# Telemetry Board Requirements

## Overview:
* The telemetry board's primary purpose is to collect, store, and send telemetry data.
	* Examples: Altitude, Temperature, Location, Orientation, etc.
* In addition to collecting data from the flight controller, the board will also have its own sensors, some sensors may be redundant.
* Wireless communication is needed for the board to send telemetry data in real time. 

## Board Requirements:
* Wireless communication using LoRa Peer to Peer (P2P)
* Data must be stored on a microSD card.
* Battery powered:
	* The battery has not been chosen, a rough guess would be a 9V to 20V battery.
* A voltage regulator to step battery voltage down to 3.3V
	* Some sensors might need 5V, in which case an additional voltage regulator will be needed for 5V power.
* A USB-C connection that can supply power, and provide serial communication between the board and a computer.
* Must be able to communicate with a flight controller.
	* From research, most flight controllers can communicate using either SPI or UART communication protocols along with needing additional GPIO pins.
	* Because the flight controller has not been selected and is subject to change, the telemetry board must have both SPI and UART connectors with additional GPIO pins.
* ESD protection (fuse, TVS diodes, etc.)
* An LED indicating power is on.
* At least one LED controlled by the MCU.
	* Will be a blinking LED.
	* Used to confirm if the MCU is working.
* Telemtry sensors (subject to change):
	* A GNSS
		* Used to track location.
	* An altitude sensor (barometric meter)
	* A temperature/humidity sensor 
	* An IMU (Inertial Measurement Unit)
* An MCU to control the board.
* A serial wire debug (SWD) connection to flash firmware on the MCU.

## PCB Constaints:
* Must be able to fit inside the glider.
	* The board area must be less than 100mm x 100mm
* Comply with [JLCPCB capabilities](https://jlcpcb.com/capabilities/Capabilities) for a 4-layer (multi-layer) board.
* Base material is FR-4.
* PCB Thickness: 1.6mm
* Minimum component size: 0603
* Use a 4-Layer PCB:
	* Inner layers are solid GND/Power planes.
	* Better for RF applications.
	* Lower EMI than 2-layer boards.
	* Easier to control impedance.
	* Reference: [2 Layer vs 4 Layer PCB: Differences, Cost, Lead Time and How to Choose](https://www.queenems.com/blog/2-layer-vs-4-layer-pcb/) 
* PCB Stackup (Top -> Bottom Layer):
	1. Signal
	2. GND
	3. GND or Power
	4. Signal  
* Layout:
	* All PCB components should be placed on the top layer.
	* Separate RF components from digital components.
* Routing:
	* Route traces on only the top or bottom layer.
	* Use orthogonal trace routing for low speed signals. See [Pros and Cons of Orthogonal Trace Routing in Multilayer PCBs](https://resources.altium.com/p/case-against-orthogonal-trace-routing-multilayer-pcbs)

## MCU Requirements:
* Use an STM32 microcontroller.
* One SPI and UART connection for the flight controller.
	* Some GPIO pins for the flight controller. 
* At least one I2C connection to interface with sensors.
* At least one SPI connection to interface with sensors.
* At least one SPI or UART connection to interface with a LoRa transceiver.
* Some GPIO pins to interface with sensors.  
* One UART connection for USB-C.
* One SPI or SDIO connection to interface with a microSD card.
* One GPIO pin to control an LED.
* Easy to solder (LQFP package type or similar)

### Estimated Minimum MCU Peripherals Needed:
* At least 3x SPI interfaces.
* At least 3x UART interfaces
* At least 1x I2C interface.
* One SDIO peripheral.
* 15+ GPIO pins.

## Main Board Components:
PCB parts are not finalized.
* One STM32 Microcontroller:
	* [STM32WL5x/Ex](https://www.st.com/en/microcontrollers-microprocessors/stm32wl5x-ex.html) 
		* Includes a built-in Lora Transceiver, but may not have enough peripherals. Max clock speed is also low, 48 MHz.
	* [STM32G4x1](https://www.st.com/en/microcontrollers-microprocessors/stm32g4x1.html)
		* Has a USB 2.0 full speed data interface, so no USB to UART Serial converter required. Does not include a Lora Transceiver.
* One external clock for the MCU.
* A 3.3V Voltage Regulator:
	* [AMS1117-3.3](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/7158/5272_AMS1117-3.3.pdf)
* USB-C:
	* One USB-C connector.
	* One USB to UART serial converter (if MCU does not have a USB interface)
* LoRa:
	* One LoRa Transceiver (SX1261, RM126x, etc).
	* One LoRa antenna (50 Ohm antenna).
* One Pressure sensor (barometric meter):
	* [MPL3115A2ST1](https://www.digikey.ca/en/products/detail/stmicroelectronics/MPL3115A2ST1/16538792)
* One Temperature sensor:
	* [LM75BIMMX-3/NOPB](https://www.digikey.ca/en/products/detail/texas-instruments/LM75BIMMX-3-NOPB/1871951)
* One IMU:
	* [LSM6DSO32TR](https://www.digikey.ca/en/products/detail/stmicroelectronics/LSM6DSO32TR/11694177?s=N4IgTCBcDaIDIGUCyA2AIgg8gZjAFQCUQBdAXyA)
* GNSS:
	* One GNSS transceiver.
	* One GNSS antenna.
* One 0805 LED
* One microSD Card Connector (SMD)
* One 2 pin connector for Power and GND.
* 2 connectors for the flight controller.
* ESD Protection:
	* At least one TVS diode.

## Reference Designs:

[1]: "STM32 PCB Design - Complete Walkthrough - Altium Designer & JLCPCB - Phil's Lab #41", Dec 5, 2021. [Online]. https://www.youtube.com/watch?v=PMEpQZ90f34

[2]: "KiCad 7 STM32 Bluetooth Hardware Design (1/2 Schematic) - Phil's Lab #127", Dec 15, 2023. [Online]. https://www.youtube.com/watch?v=PlXd3lLZ4vc
* Design tutorial for a 4-layer PCB with STM32, USB-C, and RF design.

[3]: "EcoCar Sally Prototype - RF Board", https://github.com/UAlberta-EcoCar/Sally-Electrical-Design/tree/RF-Board
* A PCB that uses LoRa and GNSS.
