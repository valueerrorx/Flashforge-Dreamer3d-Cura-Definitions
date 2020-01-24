# Flashforge-Dreamer3d-Cura-Definitions
Ultimaker Cura definitions for Flashforge Dreamer3d 


This dreamer3d.def.json works for 1 extruder only
The moment you add a second extruder Cura adds a line with the gcode
```
TO
```
right before your own startcode and Dreamer3d refuses to load the whole file.



For now this startcode should works fine with 2 extruders if you open the gcode file and remove the lines between
```
;Generated with Cura_SteamEngine 4.4.1
```
and
```
M82 ;absolute extrusion mode
```


see discussion: https://github.com/Toylerrr/Flashforge-for-Cura/issues/5

start_gcode = 
```
;Start Gcode
M140 S{material_bed_temperature};   Heat bed up to first layer temperature
M104 S{material_print_temperature} T0;   Set nozzle temperature of Right extruder to first layer temperature
M104 S{material_print_temperature} T1;   Set nozzle temperature of Left extruder to first layer temperature
M107;   Fan off
G90;   Absolute Programming
G28;   Move to Home position 
M132 X Y Z A B;   Load current home position from EEPROM
G1 Z50 F600;   Move bed down to allow safe movement of nozzles lo left front
G1 X-110.5 Y-74 F6000;   Move nozzles to left front
M7;   Wait For Platform to reach target temperature
M6 T0;   Wait For Right nozzle to reach target temperature
M6 T1;   Wait For Left nozzle to reach target temperature
; M106 enable cooling fan
M907 X100 Y100 Z40 A80 B80;   Set digital potentiometer value. A and B typically 100 for tough filament like ABS and 80 for brittle like PLA. A is Right B is Left.
G1 Z0.6 F3300; Go to start height
G4 P2000;   Dwell time
G1 Z0.2 F7200.000;   Move to first layer height
M108 T{initial_extruder_nr};   Tool change to current extruder
```


end_gcode =
```
;End Gcode
M107;   Disable cooling fan
M104 S0 T0;  Set Nozzle temperature of Right nozzle to zero
M104 S0 T1;  Set Nozzle temperature of Left nozzle to zero
M140 S0;   Turn bed heating off
G162 Z;    Home positive for Z axis
G28 X0 Y0;   Return X and Y to home position
M132 X Y Z A B
G91;   Set to relative positioning
M18;   Disable stepper motors for all axes
