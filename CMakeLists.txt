cmake_minimum_required(VERSION 3.15)
project(STM32_USB_Device_library
        VERSION 2.6.0
        DESCRIPTION "STM32L4xx USB Device Library"
        LANGUAGES C ASM)

include_guard(GLOBAL)

add_library(STM32_USB_DEVICE INTERFACE)
add_library(STM32_USB::DEVICE ALIAS STM32_USB_DEVICE)
target_include_directories(STM32_USB_DEVICE INTERFACE ${CMAKE_CURRENT_LIST_DIR}/Core/Inc)

if(NOT stm32_USB_DEBUG_LEVEL)
    set(stm32_USB_DEBUG_LEVEL 0)
    target_compile_definitions(STM32_USB_DEVICE INTERFACE USBD_DEBUG_LEVEL=${stm32_USB_DEBUG_LEVEL})
endif()

target_sources(STM32_USB_DEVICE INTERFACE
        ${CMAKE_CURRENT_LIST_DIR}/Core/Src/usbd_core.c
        ${CMAKE_CURRENT_LIST_DIR}/Core/Src/usbd_ctlreq.c
        ${CMAKE_CURRENT_LIST_DIR}/Core/Src/usbd_ioreq.c

        ${CMAKE_CURRENT_LIST_DIR}/Core/Inc/usbd_core.h
        ${CMAKE_CURRENT_LIST_DIR}/Core/Inc/usbd_ctlreq.h
        ${CMAKE_CURRENT_LIST_DIR}/Core/Inc/usbd_def.h
        ${CMAKE_CURRENT_LIST_DIR}/Core/Inc/usbd_ioreq.h
        )

add_library(STM32_USB_DEVICE_CDC INTERFACE)
add_library(STM32_USB::CDC ALIAS STM32_USB_DEVICE_CDC)
target_include_directories(STM32_USB_DEVICE_CDC INTERFACE ${CMAKE_CURRENT_LIST_DIR}/Class/CDC/Inc)
target_sources(STM32_USB_DEVICE INTERFACE ${CMAKE_CURRENT_LIST_DIR}/Class/CDC/Src/usbd_cdc.c)
