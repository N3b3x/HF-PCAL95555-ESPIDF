# HF-PCAL95555-ESPIDF

This repository packages the [HF-PCAL95555](https://github.com/N3b3x/HF-PCAL95555) driver as a reusable ESP-IDF component for the ESP32‑C6.  The upstream project provides a hardware‑agnostic C++ implementation of the NXP PCAL95555A GPIO expander.  Here it is arranged in the layout expected by the ESP‑IDF build system and exposes Kconfig options for common settings such as the default I²C address and per‑pin configuration.

## Getting Started
1. Clone this repository with submodules:
   ```bash
   git clone --recursive https://github.com/<your fork>/HF-PCAL95555-ESPIDF
   ```
2. Place it inside your application's `components/` directory or add its path via the `EXTRA_COMPONENT_DIRS` CMake variable.
3. Run `idf.py menuconfig` and configure **PCAL95555 GPIO expander** options under *Component config*.
4. Implement the `PACL95555::i2cBus` interface using the ESP‑IDF I2C APIs and instantiate `PACL95555` with the configured address.

Refer to `HF-PCAL95555/examples/esp32/main.cpp` for a minimal usage example.

## Component Layout
```
components/
  pcal95555/
    src/               # Driver source files
    Kconfig            # Configuration options imported from upstream
    CMakeLists.txt     # Component build instructions
    idf_component.yml  # Metadata for the component manager
```
The `src` directory is identical to the upstream driver and contains `pacl95555.hpp` and `pcal95555.cpp`.

## License
This component and the included HF-PCAL95555 driver are distributed under the terms of the GNU GPL v3. See the `LICENSE` file for details.
