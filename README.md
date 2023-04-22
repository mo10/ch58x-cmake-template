# ch58x-cmake-template
A simple ch58x cmake project template.

## Setup project
There are several ways to configure the toolchain
 - Install `arm-none-eabi-gcc` to `/usr/arm-none-eabi-gcc`. Project can automatically find toolchain.
 - Pass in parameter `-DCMAKE_C_COMPILER=/path/to/bin/riscv-none-embed-gcc` to cmake. 
 - Pass in parameter `-DTOOLCHAIN_PATH="/path/to/RISC-V Embedded GCC"` to cmake.
 - Set toolchain directory to `TOOLCHAIN_PATH` environment variable.

## Custom ld or startup.s
Set `TARGET_LD_SCRIPT` or `TARGET_STARTUP_ASM` as your own before `add_subdirectory(openwch)`  
See: [CMakeLists.txt](CMakeLists.txt)

## Use other version of ch58x SDK
1. Extract SDK to `openwch` directory.
2. Update `CH58X_COMMON_PATH` and `CH58X_BLE_LIB_PATH` for [CMakeLists.txt](openwch%2FCMakeLists.txt)

## License
See [LICENSE](LICENSE)

