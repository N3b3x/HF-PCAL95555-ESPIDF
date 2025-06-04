# PCAL95555 ESP-IDF Component

This component packages the [HF-PCAL95555](https://github.com/N3b3x/HF-PCAL95555) driver as an ESP-IDF component targeting the ESP32-C6.

## Features
- C++11 driver for the NXP PCAL95555A GPIO expander
- Integrates with the ESP-IDF build system
- Configurable via Kconfig (default I2C address, pin defaults, etc.)
- Uses the ESP-IDF `driver` component for I2C transactions

## Directory Structure
```
components/
  pcal95555/
    src/        # Driver sources
    Kconfig     # Configuration options
    CMakeLists.txt
    idf_component.yml
```

The original driver sources are located in `src/`. See the upstream repository for detailed API documentation.

## Usage
1. Add this repository as a submodule under your project's `components/` directory:
   ```bash
   git submodule add https://github.com/<your fork>/HF-PCAL95555-ESPIDF components/pcal95555
   git submodule update --init --recursive
   ```
2. Enable the component in menuconfig under `PCAL95555 GPIO expander`.
3. Include the header in your code:
   ```cpp
   #include "pacl95555.hpp"
   ```
4. Implement the `i2cBus` interface using ESP-IDF I2C APIs and create a `PACL95555` instance.

Example snippet:
```cpp
class ESP32I2CBus : public PACL95555::i2cBus {
public:
    bool write(uint8_t addr, uint8_t reg, const uint8_t *data, size_t len) override {
        uint8_t buf[1 + len];
        buf[0] = reg;
        memcpy(&buf[1], data, len);
        return i2c_master_write_to_device(I2C_NUM_0, addr, buf, len + 1, pdMS_TO_TICKS(100)) == ESP_OK;
    }
    bool read(uint8_t addr, uint8_t reg, uint8_t *data, size_t len) override {
        return i2c_master_write_read_device(I2C_NUM_0, addr, &reg, 1, data, len, pdMS_TO_TICKS(100)) == ESP_OK;
    }
};
```

For a full example see `HF-PCAL95555/examples/esp32/main.cpp` in the upstream repository.
