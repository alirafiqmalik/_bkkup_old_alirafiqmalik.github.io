---
layout: page
title: 2D CNC Machine (Drawbot)
description: G-code controlled drawing robot with motion planning algorithms
img: assets/img/drawbot.jpg
importance: 6
category: fun
---

This project brings digital designs into the physical world through a custom-built CNC drawing machine. By implementing G-code parsing and motion control algorithms from scratch, the system translates vector graphics into precise mechanical movements, demonstrating the fundamentals of computer-controlled manufacturing.

## System Architecture

The drawbot consists of three main subsystems working in coordination:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mechanical_assembly.jpg" title="mechanical assembly" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/stepper_control.jpg" title="stepper motor control" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/control_board.jpg" title="control electronics" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: XY gantry mechanical assembly with belt-driven stages. Center: Stepper motors providing precise positioning. Right: Arduino-based control board coordinating motion.
</div>

**Mechanical**: XY gantry system using timing belts and linear rails for smooth, accurate motion. A servo-controlled pen lift mechanism enables drawing and positioning moves.

**Electrical**: Stepper motor drivers providing the current and control signals needed to drive NEMA 17 motors. An Arduino microcontroller orchestrates all motion.

**Software**: Embedded C/C++ firmware parsing G-code commands and generating precisely timed step pulses.

## G-Code Parsing

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/gcode_example.jpg" title="G-code commands" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    G-code command structure showing movement commands, pen control, and coordinate transformations.
</div>

The firmware implements a G-code interpreter supporting standard commands:

- **G00/G01**: Rapid positioning and linear interpolation moves
- **G02/G03**: Circular interpolation (clockwise and counterclockwise arcs)
- **M03/M05**: Pen up and pen down commands
- **G90/G91**: Absolute and relative positioning modes

The parser extracts parameters from each command and validates them before execution, ensuring safe operation and catching malformed input.

## Motion Planning and Control

Translating high-level G-code commands into stepper motor movements requires sophisticated motion planning:

**Bresenham Line Algorithm**: For straight-line moves, this algorithm determines which steps to take on each axis, ensuring the mechanical path closely approximates the desired line while using only integer arithmetic.

**Acceleration Profiles**: Rather than instantly jumping to target speeds (which would cause missed steps and vibration), the system implements trapezoidal velocity profiles. Moves begin with gradual acceleration, proceed at constant velocity, then decelerate smoothly to the endpoint.

**Step Synchronization**: When both axes must move simultaneously (diagonal lines), the firmware precisely times step pulses to maintain the correct ratio of X to Y movement throughout the motion.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/motion_profile.jpg" title="velocity profile" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Trapezoidal velocity profile showing acceleration, constant velocity, and deceleration phases for smooth motion.
</div>

## Timing and Precision

Achieving precise drawings requires careful attention to timing:

**Stepper Resolution**: With 200 steps per revolution and 16x microstepping, motors provide 3,200 steps per revolution. Combined with timing belt mechanics, this yields positioning resolution of approximately 0.012mm.

**Interrupt-Driven Stepping**: Timer interrupts generate step pulses with microsecond precision, independent of other processing tasks. This ensures consistent motion even while parsing the next G-code command.

**Backlash Compensation**: Software compensation adjusts for mechanical backlash in the drive system, ensuring accuracy when changing direction.

## Python Toolchain

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/svg_to_gcode.jpg" title="conversion pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Python toolchain converting SVG vector graphics into machine-ready G-code with path optimization.
</div>

A Python toolchain bridges the gap between design software and machine control:

- **SVG Parsing**: Extracts vector paths from standard SVG files
- **Path Optimization**: Reorders paths to minimize pen-up travel time  
- **G-Code Generation**: Converts optimized paths into G-code with appropriate speeds and pen control
- **Simulation**: Visualizes the drawing path before sending to hardware

## Example Outputs

The drawbot successfully renders complex drawings including:

- Technical diagrams and CAD designs
- Typography and calligraphy
- Artistic illustrations with intricate detail
- PCB silkscreen patterns for electronics fabrication

Drawing accuracy of ±0.1mm is achieved consistently across the 300mm × 300mm work area.

## Technical Stack

- **C/C++**: Real-time motion control firmware for Arduino
- **Python**: G-code generation and path planning tools
- **Stepper Motor Control**: Timer interrupts and acceleration algorithms
- **Serial Communication**: Command streaming from PC to controller

This project demonstrates that CNC fundamentals—from motion planning to real-time control—can be mastered through hands-on implementation, providing insights applicable to industrial CNC machines, 3D printers, and other computer-controlled motion systems.