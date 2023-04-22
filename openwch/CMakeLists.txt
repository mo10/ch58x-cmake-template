cmake_minimum_required(VERSION 3.10)

set(CH58X_COMMON_PATH ${CMAKE_CURRENT_LIST_DIR}/ch583/EVT/EXAM/SRC)
set(CH58X_BLE_LIB_PATH ${CMAKE_CURRENT_LIST_DIR}/ch583/EVT/BLE/LIB)

# Peripheral Driver
file(GLOB CH58X_PERIPHERAL_SRCS ${CH58X_COMMON_PATH}/StdPeriphDriver/*.c)

# RVMSIS
set(TARGET_C_SOURCES
        ${CH58X_COMMON_PATH}/RVMSIS/core_riscv.c
        ${CH58X_PERIPHERAL_SRCS}
        )

set(TARGET_C_INCLUDES
        ${CH58X_COMMON_PATH}/RVMSIS
        ${CH58X_BLE_LIB_PATH}
        ${CH58X_COMMON_PATH}/StdPeriphDriver/inc
        )

# Shared libraries linked with application
set(TARGET_LIBS
        libISP583.a
        )

# Shared library and linker script search paths
set(TARGET_LIB_DIRECTORIES
        ${CH58X_COMMON_PATH}/StdPeriphDriver
        ${CH58X_BLE_LIB_PATH}
        )

if (NOT TARGET_LD_SCRIPT)
    set(TARGET_LD_SCRIPT ${CH58X_COMMON_PATH}/Ld/Link.ld)
    set(TARGET_LD_SCRIPT ${CH58X_COMMON_PATH}/Ld/Link.ld PARENT_SCOPE)
endif ()
message(STATUS "Use linker script: " ${TARGET_LD_SCRIPT})

if (NOT TARGET_STARTUP_ASM)
    set(TARGET_STARTUP_ASM ${CH58X_COMMON_PATH}/Startup/startup_CH583.S)
    set(TARGET_STARTUP_ASM ${CH58X_COMMON_PATH}/Startup/startup_CH583.S PARENT_SCOPE)
endif ()
message(STATUS "Use startup asm: " ${TARGET_STARTUP_ASM})

add_library(OpenWCH58x STATIC ${TARGET_C_SOURCES} ${TARGET_STARTUP_ASM})

# Shared sources, includes and definitions
target_compile_definitions(OpenWCH58x PUBLIC ${TARGET_C_DEFINES})
target_include_directories(OpenWCH58x
        PUBLIC ${TARGET_C_INCLUDES}
        INTERFACE ${TARGET_C_INCLUDES}
        )

target_link_directories(OpenWCH58x PUBLIC ${TARGET_LIB_DIRECTORIES})
target_link_libraries(OpenWCH58x PUBLIC ${TARGET_LIBS} "c" "m" "nosys")
target_link_options(OpenWCH58x
        PUBLIC -L${CH58X_BLE_LIB_PATH}/LIBCH58xBLE.a
        PUBLIC "-T${TARGET_LD_SCRIPT}"
        PUBLIC "-Wl,-Map=${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}.map,--cref"
        )