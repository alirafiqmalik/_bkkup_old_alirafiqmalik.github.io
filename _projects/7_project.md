---
layout: page
title: Voice-Controlled Home Automation System
description: ESP32-based voice automation with TinyML wake-word detection
img: assets/img/voice_automation.jpg
importance: 4
category: fun
# embedded
---

This project brings voice control to home automation through an embedded system built around the ESP32 microcontroller. By implementing on-device wake-word detection using TinyML, the system responds instantly to voice commands without requiring cloud connectivity, ensuring both privacy and reliability.

## System Overview

The automation system consists of three main components working in concert:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/esp32_system.jpg" title="ESP32 hardware" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/device_control.jpg" title="connected devices" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/web_interface.jpg" title="web dashboard" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: ESP32 development board with microphone module. Center: WiFi-controlled relay modules for device switching. Right: Responsive web dashboard for monitoring and control.
</div>

## TinyML Wake-Word Detection

The system implements local wake-word detection using machine learning models optimized for microcontroller execution. This approach offers several advantages:

**Privacy**: Audio processing happens entirely on-device—no voice data transmitted to external servers.

**Latency**: Sub-100ms wake-word detection enables near-instantaneous response.

**Reliability**: System operates independently of internet connectivity.

The neural network model was trained on custom wake-word datasets and quantized for efficient inference on the ESP32's limited computational resources. Despite running on a microcontroller, detection accuracy exceeds 90% in typical household noise conditions.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/audio_pipeline.jpg" title="audio processing pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Audio processing pipeline from microphone input through feature extraction to neural network inference.
</div>

## WiFi-Enabled Device Control

Once activated by the wake-word, the system accepts voice commands to control connected devices. The ESP32 communicates with WiFi-enabled relays and smart devices using standard protocols:

- HTTP REST APIs for device control
- MQTT messaging for event coordination
- WebSocket connections for real-time status updates

Commands are parsed locally using keyword matching and simple natural language understanding, allowing control of lights, fans, appliances, and other connected devices.

## Web Interface

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/dashboard_responsive.jpg" title="responsive dashboard" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Mobile-responsive web interface adapting to different screen sizes while maintaining full functionality.
</div>

A mobile-responsive web interface provides an alternative control method and system monitoring capabilities:

- Real-time device status display
- Manual override controls for all connected devices
- Command history and system logs
- Wake-word detection confidence visualization
- Network diagnostics and system health monitoring

The interface is built with modern web technologies, automatically adapting its layout for smartphones, tablets, and desktop browsers.

## Technical Stack

- **C/C++**: Core embedded firmware for ESP32
- **TensorFlow Lite for Microcontrollers**: ML inference engine
- **Python**: Training pipeline for wake-word detection model
- **JavaScript/HTML/CSS**: Responsive web interface
- **WebSocket**: Real-time bidirectional communication

## Key Features

- On-device ML inference requiring no cloud connectivity
- Multi-device coordination for complex automation scenarios
- Fallback manual control via web interface
- Extensible architecture supporting additional device types
- Low power consumption suitable for always-on operation

This project demonstrates that sophisticated voice control capabilities once requiring cloud infrastructure can now run entirely on affordable embedded hardware, bringing privacy-respecting voice automation within reach of DIY enthusiasts and small-scale deployments.