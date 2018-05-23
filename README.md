To use as a M5Stack component of ESP-IDF
=================================================

- Download and install [esp-idf](https://github.com/espressif/esp-idf)
- Create template idf project
  ```bash
  git clone --recursive https://github.com/m5stack/M5Stack-IDF.git
  ```
- ```make menuconfig``` has some Arduino options
  - "Autostart Arduino setup and loop on boot"
    - If you enable this options, your main.cpp should be formated like any other sketch

      ```arduino
      //file: main.cpp
      #include <M5Stack.h>

      void setup(){

        M5.begin();
        M5.Lcd.printf("hello world");
      }

      void loop() {

        M5.update();
      }
      ```

    - Else you need to implement ```app_main()``` and call ```initArduino();``` in it.

      Keep in mind that setup() and loop() will not be called in this case.
      If you plan to base your code on examples provided in [esp-idf](https://github.com/espressif/esp-idf/tree/master/examples), please make sure move the app_main() function in main.cpp from the files in the example.

      ```arduino
      //file: main.cpp
      #include <M5Stack.h>

      extern "C" void app_main()
      {
          initArduino();
          M5.begin();
          M5.Lcd.println("hello world!");
      }
      ```
  - "Disable mutex locks for HAL"
    - If enabled, there will be no protection on the drivers from concurently accessing them from another thread/interrupt/core
  - "Autoconnect WiFi on boot"
    - If enabled, WiFi will start with the last known configuration
    - Else it will wait for WiFi.begin
- ```make flash monitor``` will build, upload and open serial monitor to your board
