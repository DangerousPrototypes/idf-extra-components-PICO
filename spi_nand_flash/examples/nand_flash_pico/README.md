# SPI NAND Flash Example for Raspberry Pi Pico (RP2040)

This example demonstrates how to use the SPI NAND flash driver with the Raspberry Pi Pico (RP2040) microcontroller.

## Hardware Requirements

- Raspberry Pi Pico (or any RP2040-based board)
- SPI NAND flash chip (supported manufacturers: Winbond, GigaDevice, Alliance, Micron, Zetta, XTX)
- Connecting wires

## Wiring

| Pico Pin | Function | NAND Flash Pin |
|----------|----------|----------------|
| GP16     | SPI0 RX (MISO) | DO (Data Out) |
| GP17     | CS (GPIO) | CS# (Chip Select) |
| GP18     | SPI0 SCK | CLK (Clock) |
| GP19     | SPI0 TX (MOSI) | DI (Data In) |
| 3.3V     | Power | VCC |
| GND      | Ground | GND |

## Building

### Prerequisites

1. Install the Raspberry Pi Pico SDK:
   ```bash
   git clone https://github.com/raspberrypi/pico-sdk.git
   cd pico-sdk
   git submodule update --init
   export PICO_SDK_PATH=$(pwd)
   ```

2. Install CMake (version 3.13 or later) and a cross-compiler:
   ```bash
   # On Ubuntu/Debian
   sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi
   ```

### Build Steps

1. Navigate to the example directory:
   ```bash
   cd spi_nand_flash/examples/nand_flash_pico
   ```

2. Create a build directory and run CMake:
   ```bash
   mkdir build
   cd build
   cmake ..
   ```

3. Build the project:
   ```bash
   make -j4
   ```

4. The build will produce `nand_flash_pico.uf2` in the build directory.

## Flashing

1. Hold the BOOTSEL button on the Pico and connect it to your computer via USB.
2. The Pico will appear as a mass storage device.
3. Copy the `nand_flash_pico.uf2` file to the Pico drive.
4. The Pico will automatically reboot and run the program.

## Output

The example outputs to USB serial (CDC). Connect to the serial port at 115200 baud to see the output:

```
[I][example] SPI NAND Flash Example for Raspberry Pi Pico
[I][example] ==============================================
[I][example] SPI initialized at 40000000 Hz
[I][example] NAND flash initialized successfully
[I][example] Flash Info:
[I][example]   Number of blocks: 1024
[I][example]   Block size: 131072 bytes
[I][example]   Sector size: 2048 bytes
[I][example]   Total capacity: 65536 sectors
[I][example]   Total size: 131072 KB
[I][example] Writing test data to sector 0...
[I][example] Write successful
[I][example] Reading data from sector 0...
[I][example] Read successful
[I][example] Read data: 'Hello from Raspberry Pi Pico!'
[I][example] Data verification: PASSED
[I][example] Example complete!
```

## Supported NAND Flash Devices

The driver supports the following manufacturers and device families:

- **Winbond**: W25N series (512MB - 4GB)
- **GigaDevice**: GD5F series (1Gbit - 4Gbit)
- **Alliance**: AS5F series (1Gbit - 8Gbit)
- **Micron**: MT29F series (1Gbit - 4Gbit)
- **Zetta**: ZD35 series
- **XTX**: XT26G series

## Notes

- This port uses Single I/O (SIO) mode only. Dual I/O and Quad I/O modes are not supported on standard RP2040 SPI.
- The Dhara FTL (Flash Translation Layer) is used for wear-leveling and bad block management.
- The default SPI frequency is 40 MHz. Adjust `FLASH_FREQ_HZ` in `main.c` if needed for your specific flash chip.

## Pin Configuration

To change the default pin configuration, modify these defines in `main.c`:

```c
#define SPI_PORT spi0
#define PIN_MISO 16  // GP16 - SPI0 RX
#define PIN_CS   17  // GP17 - Chip Select
#define PIN_CLK  18  // GP18 - SPI0 SCK
#define PIN_MOSI 19  // GP19 - SPI0 TX
```

You can also use SPI1 with different pins:

```c
#define SPI_PORT spi1
#define PIN_MISO 12  // GP12 - SPI1 RX
#define PIN_CS   13  // GP13 - Chip Select
#define PIN_CLK  14  // GP14 - SPI1 SCK
#define PIN_MOSI 15  // GP15 - SPI1 TX
```

## License

This example is licensed under the Apache 2.0 License or the Unlicense (public domain), at your choice.
