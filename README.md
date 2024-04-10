# 3DChameleon-Klipper

## Introduction
The 3D Chameleon is a Multimaterial system that allows 3D Printers to print in upto 4 colours. These Klipper macros aim to make the 3DChameleon compatible with Klipper firmware. By integrating Macros defined in Klipper, it enables seamless switching and handling of multiple filaments during printing.
https://www.3dchameleon.com/

## Usage
1. **Wiring**
  Plug the two stepper motors of the 3DChameleon into any two free stepper motor slots on your Motherboard. 
  
1. **Add Configuration File:**
   Add `mmu_macros.cfg` to your 3D printer's configuration files.

2. **Update Printer Configuration:**
  - Include `mmu_macros.cfg` in your `printer.cfg` file by adding this line to your `printer.cfg`.
      ```
      [include mmu_macros.cfg]
      ```
  - Add sections for the additional stepper drives in your `printer.cfg`.
      Sample config for `Octopus Pro v1.1 H723`
      ```
      # MMU Drivers
      [tmc2209 manual_stepper mmu_extruder_stepper]
      uart_pin: PC4
      diag_pin: PG6
      run_current: 0.600
      stealthchop_threshold: 999999
      
      [tmc2209 manual_stepper mmu_selector_stepper]
      uart_pin: PD11
      diag_pin: PG9
      run_current: 0.600
      stealthchop_threshold: 999999
      driver_sgthrs:20
  
      [manual_stepper mmu_extruder_stepper]
      step_pin: PF13
      dir_pin: PF12
      enable_pin: !PF14
      endstop_pin: tmc2209_mmu_extruder_stepper:virtual_endstop
      microsteps: 16
      rotation_distance: 40
      homing_speed: 50
      
      #[filament_switch_sensor material_2]
      #switch_pin: PG14
      
      # Driver7
      [manual_stepper mmu_selector_stepper]
      step_pin: PG0
      dir_pin: PG1
      enable_pin: !PF15
      microsteps: 16
      rotation_distance: 40
      endstop_pin: tmc2209_mmu_selector_stepper:virtual_endstop
      homing_speed: 20
  
      [mcu]
      serial: /dev/serial/by-id/usb-Klipper_stm32h723xx_12345-if00
      
      [virtual_sdcard]
      path: /home/pi/printer_data/gcodes
      on_error_gcode: CANCEL_PRINT
      
      [printer]
      kinematics: none
      max_velocity: 300
      max_accel: 3000
      ```
3. **Slicer Configuration:**
   Configure your slicer to specify the locations where filament changes will occur using the `MMU_SWITCH_FILAMENT {Filament_no}` command.
