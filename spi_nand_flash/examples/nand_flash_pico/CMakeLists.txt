# SPDX-FileCopyrightText: 2024 Espressif Systems (Shanghai) CO LTD
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.13)

# Include pico_sdk_import.cmake before project()
include(pico_sdk_import.cmake)

project(nand_flash_pico C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the Raspberry Pi Pico SDK
pico_sdk_init()

# Define the root of spi_nand_flash component relative to this example
set(SPI_NAND_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../..)
set(DHARA_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../../../dhara)

# Add dhara library
add_library(dhara STATIC
    ${DHARA_ROOT}/dhara/map.c
    ${DHARA_ROOT}/dhara/journal.c
    ${DHARA_ROOT}/dhara/error.c
)

target_include_directories(dhara PUBLIC
    ${DHARA_ROOT}
)

# Add spi_nand_flash library for PICO
add_library(spi_nand_flash STATIC
    src/nand_pico.c
    src/dhara_glue_pico.c
    src/nand_impl_pico.c
    src/spi_nand_oper_pico.c
    src/nand_winbond_pico.c
    src/nand_gigadevice_pico.c
    src/nand_alliance_pico.c
    src/nand_micron_pico.c
    src/nand_zetta_pico.c
    src/nand_xtx_pico.c
)

target_include_directories(spi_nand_flash PUBLIC
    include
    ${DHARA_ROOT}
)

target_link_libraries(spi_nand_flash
    pico_stdlib
    hardware_spi
    dhara
)

# Add main executable
add_executable(nand_flash_pico
    main.c
)

target_link_libraries(nand_flash_pico
    pico_stdlib
    hardware_spi
    spi_nand_flash
)

# Enable USB output, disable UART output
pico_enable_stdio_usb(nand_flash_pico 1)
pico_enable_stdio_uart(nand_flash_pico 0)

# Create map/bin/hex/uf2 files
pico_add_extra_outputs(nand_flash_pico)
