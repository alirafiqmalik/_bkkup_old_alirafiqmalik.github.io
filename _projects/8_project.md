---
layout: page
title: Scalar RISC-V Microprocessor
description: 5-stage pipelined processor with hazard detection and forwarding
img: assets/img/riscv_processor.jpg
importance: 3
category: fun
# hardware
---

This project implements a complete RISC-V processor core from scratch, demonstrating the principles of pipelined computer architecture. The design achieves instruction-level parallelism through a classic 5-stage pipeline while maintaining correctness through sophisticated hazard detection and data forwarding mechanisms.

## Pipeline Architecture

The processor implements the traditional five-stage pipeline:

1. **Instruction Fetch (IF)**: Retrieves instructions from memory
2. **Instruction Decode (ID)**: Decodes instructions and reads register file
3. **Execute (EX)**: Performs ALU operations and address calculations
4. **Memory (MEM)**: Accesses data memory for loads and stores
5. **Write Back (WB)**: Writes results back to register file

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pipeline_diagram.jpg" title="pipeline stages" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/datapath.jpg" title="processor datapath" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Five-stage pipeline visualization showing instruction flow. Right: Complete processor datapath with control signals.
</div>

## Hazard Detection and Resolution

Pipelining introduces data, control, and structural hazards that must be resolved to maintain program correctness:

**Data Hazards**: Detected when an instruction depends on results from previous instructions still in the pipeline. The forwarding unit resolves most hazards by routing data directly from pipeline registers, avoiding stalls.

**Control Hazards**: Branch and jump instructions require special handling. The design implements branch prediction and uses the ALU in the EX stage for early branch resolution, minimizing pipeline flushes.

**Load-Use Hazards**: When a load instruction is immediately followed by an instruction using the loaded data, a pipeline stall is unavoidable. The hazard detection unit identifies these cases and inserts bubbles appropriately.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/forwarding_paths.jpg" title="data forwarding paths" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Data forwarding paths enabling hazard-free execution in most cases, with forwarding from EX/MEM and MEM/WB stages.
</div>

## Testing and Verification

A comprehensive verification strategy ensures processor correctness:

**Randomized Test Generation**: Python scripts generate randomized instruction sequences covering all instruction types, edge cases, and hazard scenarios. This approach reveals corner cases that manual test writing might miss.

**Waveform Analysis**: Detailed signal tracing through simulation enables verification of control signals, data paths, and timing at the cycle level. Each pipeline stage's behavior is validated against expected results.

**ISA Compliance**: The processor is verified against RISC-V ISA specifications, ensuring correct execution of the RV32I base integer instruction set.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/waveform_analysis.jpg" title="simulation waveforms" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Waveform analysis showing pipeline register contents and control signals across multiple clock cycles.
</div>

## Implementation Details

- **Verilog HDL**: Complete processor implementation in synthesizable Verilog
- **Modular Design**: Separate modules for each pipeline stage, control unit, ALU, and register file
- **Python Tooling**: Test generation and result verification scripts
- **Makefile Build System**: Automated simulation and testing workflow
- **Shell Scripts**: Batch testing and regression verification

The project demonstrates that even complex microarchitectural features like pipelining and forwarding can be implemented cleanly with careful design and thorough verification. The resulting processor achieves near-ideal CPI (cycles per instruction) for hazard-free code sequences.
