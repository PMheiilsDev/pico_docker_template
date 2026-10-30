# Pico Blink

Minimal Raspberry Pi Pico (RP2040) project using the Pico C/C++ SDK.
GPIO25 turns on for 500 ms and off for 500 ms. On a standard Pico this
drives the onboard LED; Pico W's onboard LED is not connected to GPIO25.

## Docker / VS Code Dev Container

In VS Code, run **Dev Containers: Rebuild and Reopen in Container**.
The container includes the Pico SDK, ARM toolchain, CMake, OpenOCD, and GDB.

In the container terminal:

```sh
cmake -S . -B build -DPICO_BOARD=pico
cmake --build build
```

Alternatively, build without VS Code:

```sh
docker build -t pico-dev .
docker run --rm --user "$(id -u):$(id -g)" -v "$PWD:/workspace" -w /workspace pico-dev \
	cmake -S . -B build -DPICO_BOARD=pico
docker run --rm --user "$(id -u):$(id -g)" -v "$PWD:/workspace" -w /workspace pico-dev \
	cmake --build build
```

Use a fresh build directory when switching between host and container builds.

## Native Build

Install CMake, the Arm GNU toolchain (`arm-none-eabi-gcc`), and the
[Pico SDK](https://github.com/raspberrypi/pico-sdk).
Replace `/path/to/pico-sdk` below with your SDK directory.

```sh
cmake -S . -B build -DPICO_BOARD=pico -DPICO_SDK_PATH=/path/to/pico-sdk
cmake --build build
```

## Flash

Hold BOOTSEL while connecting the Pico over USB, then copy
`build/pico_blink.uf2` to the RPI-RP2 drive.

## Debug

Connect a CMSIS-DAP probe to the Pico's SWD pins and select
**Pico Debug (Docker)** in VS Code inside the dev container.
The dev container passes through USB devices on Linux and runs privileged
for probe access. Remove those run arguments if hardware access is not needed.

## Customize

Change `led_pin` or the `sleep_ms` delays in `main.c` to start your project.