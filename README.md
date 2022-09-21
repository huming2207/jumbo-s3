# jumbo-s3

PoC hardware design of ESP32-S3FN8 + 16MB (APS12808L-3OBM-BA) PSRAM

## Intro

As of September 2022, Espressif doesn't provide an official solution for ESP32-S3 that comes with more than 8MB PSRAM. However someone mentioned that the APS12808L works with ESP32-S3 but no open source design.

Therefore here comes my solution. It has been proven working fine. No modification needed for ESP-IDF. As long as the PSRAM support is enabled and set to octal mode, 80MHz, it should just works.

## Log

Here's a simple boot log that runs the APS12808L in octal SPI mode at 80MHz, and logs the free heap size.

```
ESP-ROM:esp32s3-20210327
Build:Mar 27 2021
rst:0x1 (POWERON),boot:0x8 (SPI_FAST_FLASH_BOOT)
SPIWP:0xee
mode:DIO, clock div:1
load:0x3fce3810,len:0x1808
load:0x403c9700,len:0xd6c
load:0x403cc700,len:0x2ed4
entry 0x403c992c
I (24) boot: ESP-IDF v5.1-dev-862-g09f7589ef2 2nd stage bootloader
I (25) boot: compile time Sep 20 2022 23:02:14
I (25) boot: chip revision: V001
I (29) boot_comm: chip revision: 1, min. bootloader chip revision: 0
I (36) qio_mode: Enabling default flash chip QIO
I (42) boot.esp32s3: Boot SPI Speed : 80MHz
I (46) boot.esp32s3: SPI Mode       : QIO
I (51) boot.esp32s3: SPI Flash Size : 4MB
I (56) boot: Enabling RNG early entropy source...
I (61) boot: Partition Table:
I (65) boot: ## Label            Usage          Type ST Offset   Length
I (72) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (79) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (87) boot:  2 factory          factory app      00 00 00010000 00100000
I (94) boot: End of partition table
I (99) boot_comm: chip revision: 1, min. application chip revision: 0
I (106) esp_image: segment 0: paddr=00010020 vaddr=3c020020 size=08a5ch ( 35420) map
I (120) esp_image: segment 1: paddr=00018a84 vaddr=3fc92700 size=03268h ( 12904) load
I (125) esp_image: segment 2: paddr=0001bcf4 vaddr=40374000 size=04324h ( 17188) load
I (135) esp_image: segment 3: paddr=00020020 vaddr=42000020 size=196fch (104188) map
I (155) esp_image: segment 4: paddr=00039724 vaddr=40378324 size=0a30ch ( 41740) load
I (164) esp_image: segment 5: paddr=00043a38 vaddr=50000000 size=00010h (    16) load
I (170) boot: Loaded app from partition at offset 0x10000
I (170) boot: Disabling RNG early entropy source...
I (184) octal_psram: vendor id    : 0x0d (AP)
I (184) octal_psram: dev id       : 0x02 (generation 3)
I (184) octal_psram: density      : 0x05 (128 Mbit)
I (189) octal_psram: good-die     : 0x01 (Pass)
I (195) octal_psram: Latency      : 0x01 (Fixed)
I (200) octal_psram: VCC          : 0x01 (3V)
I (205) octal_psram: SRF          : 0x01 (Fast Refresh)
I (211) octal_psram: BurstType    : 0x01 (Hybrid Wrap)
I (217) octal_psram: BurstLen     : 0x01 (32 Byte)
I (222) octal_psram: Readlatency  : 0x02 (10 cycles@Fixed)
I (228) octal_psram: DriveStrength: 0x00 (1/1)
W (233) PSRAM: DO NOT USE FOR MASS PRODUCTION! Timing parameters will be updated in future IDF version.
I (244) esp_psram: Found 16MB PSRAM device
I (248) esp_psram: Speed: 80MHz
I (252) cpu_start: Pro cpu up.
I (256) cpu_start: Starting app cpu, entry point is 0x40375378
I (0) cpu_start: App cpu up.
I (1167) esp_psram: SPI SRAM memory test OK
I (1176) cpu_start: Pro cpu start user code
I (1176) cpu_start: cpu freq: 160000000 Hz
I (1176) cpu_start: Application information:
I (1179) cpu_start: Project name:     main
I (1184) cpu_start: App version:      1
I (1188) cpu_start: Compile time:     Sep 20 2022 23:02:12
I (1195) cpu_start: ELF file SHA256:  1f3449a15147ac7a...
Warning: checksum mismatch between flashed and built applications. Checksum of built application is e6bd8ac9c4e022dd86afd168d00707482f76b53ee256de9e101a4846cd271af5
I (1201) cpu_start: ESP-IDF:          v5.1-dev-862-g09f7589ef2
I (1207) heap_init: Initializing. RAM available for dynamic allocation:
I (1215) heap_init: At 3FC963E8 len 00053328 (332 KiB): D/IRAM
I (1221) heap_init: At 3FCE9710 len 00005724 (21 KiB): STACK/DRAM
I (1228) heap_init: At 3FCF0000 len 00008000 (32 KiB): DRAM
I (1234) heap_init: At 600FE010 len 00001FF0 (7 KiB): RTCRAM
I (1241) esp_psram: Adding pool of 16384K of PSRAM memory to heap allocator
I (1249) spi_flash: detected chip: gd
I (1253) spi_flash: flash io: qio
W (1257) spi_flash: Detected size(8192k) larger than the size in the binary image header(4096k). Using the size in the binary image header.
I (1270) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (1290) esp_psram: Reserving pool of 32K of internal memory for DMA/internal allocations
I (1290) test: Lay hou
I (1300) test: Max heap: 17117304

```
