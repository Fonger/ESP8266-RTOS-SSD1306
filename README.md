# ESP8266-RTOS-SSD1306

SSD1306/SH1106 components for ESP8266_RTOS_SDK. This is a direct port from [extras/ssd1306](https://github.com/SuperHouse/esp-open-rtos/tree/master/extras/ssd1306) component from [esp-open-rtos](https://github.com/SuperHouse/esp-open-rtos).

However, only i2s protocol is supported. If I have SPI version, I'll try to port and test it.

You can use this module along with [ESP8266-RTOS-FONTS](https://github.com/Fonger/ESP8266-RTOS-FONTS) to display text to oled display.

## Compatibility

- [espressif/ESP8266_RTOS_SDK](https://github.com/espressif/ESP8266_RTOS_SDK) v3.2+ (esp-idf style)
- ssd1306/sh1106 oled display with i2s protocol

## Usage

Clone this into your project `components` folder.

```c

#include <stdio.h>

#include <freertos/FreeRTOS.h>
#include <freertos/task.h>
#include <driver/gpio.h>

#include <ssd1306/ssd1306.h>
#include <driver/i2c.h>
#include <esp_err.h>

#define SCL_PIN 5
#define SDA_PIN 4
#define DISPLAY_WIDTH 128
#define DISPLAY_HEIGHT 64

void app_main()
{
    // init i2s
    int i2c_master_port = I2C_NUM_0;
    i2c_config_t conf;
    conf.mode = I2C_MODE_MASTER;
    conf.sda_io_num = SDA_PIN;
    conf.sda_pullup_en = 1;
    conf.scl_io_num = SCL_PIN;
    conf.scl_pullup_en = 1;
    conf.clk_stretch_tick = 300;
    ESP_ERROR_CHECK(i2c_driver_install(i2c_master_port, conf.mode));
    ESP_ERROR_CHECK(i2c_param_config(i2c_master_port, &conf));

    // init ssd1306
    ssd1306_t dev = {
        .i2c_port = i2c_master_port,
        .i2c_addr = SSD1306_I2C_ADDR_0,
        .screen = SSD1306_SCREEN, // or SH1106_SCREEN
        .width = DISPLAY_WIDTH,
        .height = DISPLAY_HEIGHT};

    ssd1306_init(&dev);

}
```
