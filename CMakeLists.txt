cmake_minimum_required(VERSION 3.10)
include(cmake/toolchain.cmake)

project(ch58x)

enable_language(C)
enable_language(CXX)
enable_language(ASM)

# Custom linker script
#set(TARGET_LD_SCRIPT ${CMAKE_SOURCE_DIR}/ld/Link.ld)
# Custom startup
#set(TARGET_STARTUP_ASM ${CMAKE_SOURCE_DIR}/startup/startup_CH583.S)

# Add OpenWCH SDK
add_subdirectory(openwch)

# Add project sources
add_executable(ch58x src/main.c)

target_link_libraries(ch58x OpenWCH58x)
# Generate .bin and .hex
generate_binary_file(ch58x)
generate_hex_file(ch58x)
