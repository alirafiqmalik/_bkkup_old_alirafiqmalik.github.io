---
layout: page
title: Multi-Agent Aerial Swarm Coordination
description: Event-driven coordination protocol for autonomous drone swarms
img: assets/img/drone_swarm.jpg
importance: 1
category: work
related_publications: true
---

As an Undergraduate Research Assistant at the Communication Systems and Networks Lab, I collaborated on developing cutting-edge coordination protocols for multi-agent aerial swarms. This project focused on enabling autonomous drones to work together seamlessly in complex formations and dynamic environments.

## System Architecture

The core of this project involved implementing an event-driven coordination protocol on Raspberry Pi companion computers integrated with Pixhawk/ArduPilot flight controllers. This architecture provided the computational power needed for real-time decision-making while maintaining the reliability of proven flight control systems.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/swarm_flock.jpg" title="Flock formation" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/swarm_line.jpg" title="Line formation" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/swarm_helical.jpg" title="Helical formation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Three primary formation types implemented: flock formation for coordinated group movement, line formation for sequential operations, and helical formation for complex aerial maneuvers.
</div>

## Formation Control Design

I designed and optimized leader-follower formation control algorithms supporting three distinct formation types: flock, line, and helical configurations. A key achievement was enabling dynamic reconfiguration between formations with under 2 minutes of switching latency, allowing the swarm to adapt to changing mission requirements in near real-time.

The formation control system utilized distributed consensus algorithms where follower agents maintained relative positions to the leader while accounting for inter-agent collision avoidance. Each formation type was parameterized to allow for scalable swarm sizes and adjustable spacing constraints.

## Fault-Tolerant Communication Stack

One of the most challenging aspects was engineering a robust mesh networking stack to ensure reliable communication between agents. The system leveraged IEEE 802.11 for wireless connectivity, implementing both UDP for time-critical control messages and TCP for reliable data transfer. Integration with the MAVLink protocol enabled standardized communication with the flight controllers.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/network_topology.jpg" title="Mesh network topology" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/latency_graph.jpg" title="Communication latency" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The mesh networking architecture (left) ensured redundant communication paths, while latency measurements (right) confirmed real-time performance under 100ms.
</div>

The fault-tolerant design included automatic route discovery and healing mechanisms, ensuring that the loss of individual communication links wouldn't compromise the entire swarm's coordination. Through careful optimization, we achieved communication latencies under 100ms, critical for maintaining stable formation control.

## Impact and Applications

This research has applications in disaster response, agricultural monitoring, search and rescue operations, and coordinated surveillance. The event-driven architecture ensures efficient resource utilization while the dynamic reconfiguration capability allows swarms to adapt to evolving mission parameters without returning to base.

**Duration:** September 2022 - July 2023  
**Institution:** National University of Sciences & Technology, Pakistan  
**Lab:** Communication Systems and Networks Lab