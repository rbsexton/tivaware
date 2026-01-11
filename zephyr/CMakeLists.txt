# TivaWare CMakeLists.txt for Zephyr integration
# Copyright (c) 2025 Robert Sexton
# SPDX-License-Identifier: Apache-2.0

# Only build TivaWare if TM4C SoC is selected
if(CONFIG_SOC_SERIES_TM4C129X OR CONFIG_SOC_TI_TM4C129XNCZAD)

zephyr_library()

# Include directories - paths relative to module root
zephyr_library_include_directories(
  ${ZEPHYR_CURRENT_MODULE_DIR}
  ${ZEPHYR_CURRENT_MODULE_DIR}/inc
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib
)

# Core system files (always included) - paths relative to module root
zephyr_library_sources(
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/cpu.c
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/interrupt.c
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/sysctl.c
)

# Optional: FPU support (for Cortex-M4F)
zephyr_library_sources_ifdef(CONFIG_FPU
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/fpu.c
)

# GPIO driver
zephyr_library_sources_ifdef(CONFIG_GPIO
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/gpio.c
)

# UART driver (Tiva C only - Stellaris uses uart_stellaris.c)
zephyr_library_sources_ifdef(CONFIG_UART_TIVAC
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/uart.c
)

# Ethernet driver (for both Stellaris and TiVa-C)
zephyr_library_sources_ifdef(CONFIG_ETH_STELLARIS
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/emac.c
)
zephyr_library_sources_ifdef(CONFIG_ETH_TIVAC
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/emac.c
)

# Flash driver
zephyr_library_sources_ifdef(CONFIG_FLASH
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/flash.c
)

# EEPROM driver
zephyr_library_sources_ifdef(CONFIG_EEPROM
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/eeprom.c
)

# ADC driver
zephyr_library_sources_ifdef(CONFIG_ADC
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/adc.c
)

# I2C driver
zephyr_library_sources_ifdef(CONFIG_I2C
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/i2c.c
)

# SPI driver
zephyr_library_sources_ifdef(CONFIG_SPI
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/ssi.c
)

# PWM driver
zephyr_library_sources_ifdef(CONFIG_PWM
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/pwm.c
)

# Timer driver
zephyr_library_sources_ifdef(CONFIG_COUNTER
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/timer.c
)

# Watchdog driver
zephyr_library_sources_ifdef(CONFIG_WATCHDOG
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/watchdog.c
)

# CAN driver
zephyr_library_sources_ifdef(CONFIG_CAN
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/can.c
)

# USB driver
zephyr_library_sources_ifdef(CONFIG_USB_DEVICE_DRIVER
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/usb.c
)

# DMA driver
zephyr_library_sources_ifdef(CONFIG_DMA
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/udma.c
)

# Systick (system timer)
zephyr_library_sources_ifdef(CONFIG_CORTEX_M_SYSTICK
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/systick.c
)

# Crypto drivers
zephyr_library_sources_ifdef(CONFIG_CRYPTO
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/aes.c
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/des.c
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/shamd5.c
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/crc.c
)

# Hibernate module
zephyr_library_sources_ifdef(CONFIG_PM
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/hibernate.c
)

# System exception handling
zephyr_library_sources(
  ${ZEPHYR_CURRENT_MODULE_DIR}/driverlib/sysexc.c
)

# Compiler-specific definitions
zephyr_library_compile_definitions(
  TARGET_IS_TM4C129_RA2
  PART_TM4C129XNCZAD
)

# Export include directories for other modules
# Export one level up from module root so code can use <tivaware/inc/hw_uart.h>
# ZEPHYR_CURRENT_MODULE_DIR points to ~/projects/tivaware
# We need to export its parent so tivaware/ prefix works
get_filename_component(TIVAWARE_PARENT_DIR ${ZEPHYR_CURRENT_MODULE_DIR} DIRECTORY)
zephyr_include_directories(${TIVAWARE_PARENT_DIR})


endif() # CONFIG_SOC_SERIES_TM4C129X OR CONFIG_SOC_TI_TM4C129XNCZAD
