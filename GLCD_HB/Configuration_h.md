# HB: Configuration.h


## Instructions


## Code
```

#define STRING_CONFIG_H_AUTHOR "Vijay Raghav"
#define SHOW_BOOTSCREEN
#define STRING_SPLASH_LINE1 0.01
#define STRING_SPLASH_LINE2 "www.fracktal.in" 

#define SHOW_CUSTOM_BOOTSCREEN

#define BAUDRATE 115200

#define MOTHERBOARD BOARD_RAMPS_13_EFB
#define CUSTOM_MACHINE_NAME "Julia"

#define DEFAULT_NOMINAL_FILAMENT_DIA 1.75

#define TEMP_SENSOR_0 3
#define TEMP_SENSOR_BED 3

#define TEMP_RESIDENCY_TIME 1
#define TEMP_HYSTERESIS 4

#define TEMP_BED_RESIDENCY_TIME 2

#define  DEFAULT_Kp 42.96
#define  DEFAULT_Ki 5.14
#define  DEFAULT_Kd 89.73

#define COREXY

//#define USE_YMIN_PLUG
//#define USE_ZMIN_PLUG

#define USE_YMAX_PLUG
#define USE_ZMAX_PLUG

#define ENDSTOPPULLUP_ZMAX
#define ENDSTOPPULLUP_XMIN
#define ENDSTOPPULLUP_YMIN


#define DEFAULT_AXIS_STEPS_PER_UNIT   { 160,  160, 1007.874, 280 }


#define DEFAULT_MAX_FEEDRATE          { 200, 200, 20, 45 }


#define DEFAULT_MAX_ACCELERATION      { 1000, 1000, 50, 10000 }

#define DEFAULT_ACCELERATION          1000    // X, Y, Z and E acceleration for printing moves
#define DEFAULT_RETRACT_ACCELERATION  2000    // E acceleration for retracts
#define DEFAULT_TRAVEL_ACCELERATION   1000    // X, Y, Z acceleration for travel (non printing) moves


#define DEFAULT_XJERK                  5.0
#define DEFAULT_YJERK                  5.0
#define DEFAULT_ZJERK                  0.4

//#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN

#define INVERT_Y_DIR false

#define INVERT_E0_DIR true
#define INVERT_E1_DIR true

#define Y_HOME_DIR 1
#define Z_HOME_DIR 1

#define X_MAX_POS 210
#define Y_MAX_POS 200
#define Z_MAX_POS 210

#define GLCD_BED_LEVELING

#define UBL_G26_MESH_VALIDATION

#define HOMING_FEEDRATE_Z  (20*60)

#define EEPROM_SETTINGS

#define NOZZLE_PARK_FEATURE

#define PRINTCOUNTER

#define SDSUPPORT

#define ENCODER_PULSES_PER_STEP 2

#define ENCODER_STEPS_PER_MENU_ITEM 2

#define SPEAKER

#define MKS_MINI_12864

```
