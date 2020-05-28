# Mac driver and daemon for Thermaltake Riing
Forked from https://github.com/chestm007/linux_thermaltake_riing

This is a pure copy adapted to work on Mac using osx-cpu-temp as temperature sensor

WARNING: there's a lot of WiP here, im not a Python developer, so maybe things here look strange.

Help and PR's are welcome!

## Compatibility
Python3 only.

Currently supported devices are (as they show up in thermaltakes TTRGBPLUS software:  
- Flow Riing RGB  
- Lumi Plus LED Strip  
- Pacific PR22-D5 Plus  
- Pacific Rad Plus LED Panel  
- Pacific V-GTX 1080Ti Plus GPU Waterblock  
- Pacific W4 Plus CPU Waterblock  
- Riing Plus  

If your's isn't listed, please create an issue and I'll implement it ASAP!!  


## Installation
- osx-cpu-temp (`brew install osx-cpu-temp`)
- libusb (`brew install libusb`)
- pyyaml (`pip3 install pyyaml`)
- GObject (`pip3 install GObject`)
- pyusb (`pip3 install pyusb`)
- numpy (`pip3 install numpy`)
- pickle5 (`pip3 install pickle5`)


## Run
Launch `python3 run.py` on the main project folder.

Daemon is not available, as I don't know how to do it under Mac OS
## Configuration
the configuration file is expected to be in: `(user_home)/mac_thermaltake_rgb/config.yml`  
edit and configure suitably.  

example config is in `mac_thermaltake_rgb/assets/config.yml`  

### Controller Types
- g3
- riingtrio

### Devices

- Fans
  - Riing Plus
  
- Lights
  - Pacific PR22-D5 Plus
  - Pacific W4 Plus CPU Waterblock
  - Pacific V-GTX 1080Ti Plus GPU Waterblock
  - Pacific Rad Plus LED Panel
  - Lumi Plus LED Strip
  
- Pumps
  - Floe Riing RGB

### Fan Manager Settings

- temp_target
  increases/decreases fan speed in direct response to the difference of actual
  temperature to target temperature multiplied by the specified multiplier
  with an extemely simple - read, dumb - smoothing function
  - settings:
    - target [target temperature]
    - multiplier
    
- locked_speed
  sets the fan speeds to a static speed, regardless of temperature 
  or... anything really
  - settings:
    - speed [0-100]
    
- curve
  allows defining of a fan speed curve
  - settings:
    - points  
      example config:
    
    ```yaml
    fan_manager:
      model: curve
      points:
        - [0, 0]  # [temp(*C), speed(0-100%)]
        - [50, 30]
        - [70, 100]

    ```
    
### Lighting Manager Settings
To save repetition:  
speed: desired refresh speed of the device ['slow', 'normal', 'fast', 'extreme']
g/r/b: RGB values of the desired colour

- alternating  
  alternates between odd_rgb and even_rgb such that every "tick" the lights
  on the device will alternate between the 2 colours
  - settings:  
    - speed  
    - odd_rgb:  
      - g  
      - r  
      - b  
    - even_rgb:  
      - g  
      - r  
      - b  
      
- temperature 
  will set lighting to a colour between blue/green/red and anywhere inbetween
  depending on the temperature of the selected sensor
  - settings: 
    - speed 
    - cold [desired temperature to set lighting to blue]
    - hot [desired temperature to set lighting to red]
    - target [desired temperature to set lighting to green]

- full 
  sets lighting to this colour
  - settings: 
    - r 
    - g 
    - b 
    
- off 
  as it sounds, turns lighting off
  - settings: 

- flow 
  walks each led in the device slowly going around the RGB spectrum individually
  - settings: 
    - speed 
    
- spectrum 
  fades all lights at the same time through the RGB spectrum
  - settings: 
    - speed 
    
- ripple 
  leading led light followed by a trail of fading led's that walk all led's in
  the device
  - settings: 
    - speed 
    - r 
    - g 
    - b 
    
- blink 
  repeatedly flashes the led's in the devices in the selected colour
  - settings: 
    - speed 
    - r 
    - g 
    - b 
    
- pulse 
  same as blink, except using a smooth fade
  - settings: 
    - speed 
    - r 
    - g 
    - b 
  

