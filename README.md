


# Neuromorphic LIF Neuron (ngspice)
## Overview
This repository implements a Leaky Integrate-and-Fire (LIF) neuron at circuit level in NGSpice. The model captures how a neuron:
* Integrates input over time
* Leaks charge
* Fires discrete spike events at a threshold
* Resets and enforces a refractory period
The goal is to provide a stable, circuit-aware neuromorphic primitive suitable for studying firing-rate control, sparsity, and event-driven behavior.

## What’s Implemented
* Continuous-time membrane integration (RC)
* Behavioural leak current (device-aware abstraction)
* Soft threshold detection (numerically stable)
* Hard reset to a reset voltage
* Refractory period using RC dynamics
This yields robust, repeatable tonic spiking and clean spike events.

## Circuit Blocks
### Membrane (Integrator)
Stores charge and integrates input:
Cmem vm 0 {Cmem}
### Input Drive (Synaptic Current)
Converts voltage to current:
Vin in 0 DC {VinDC}
Rin in vm {Rin}
### Leak (Passive Decay)
Voltage-dependent leak current:
Bleak vm 0 I = { G_leak * V(vm) }
### Threshold & Spike Generation
Soft comparator for stable spike detection:
VTH th 0 {Vth}
BSPK spk_raw 0 V = { 0.5*(1 + tanh((V(vm)-V(th))/{eps})) }
### Reset & Refractory
Hard reset and RC-based refractory hold:
RLP spk_raw refrac {Rref}
CLP refrac 0 {Cref}
VRESET vreset_node 0 {Vreset}
SRESET vm vreset_node refrac 0 SWRST

## How to Run
ngspice lif_neuron.cir
Observe:
* vm → membrane potential (sawtooth)
* spk_raw → spike events
* refrac → refractory envelope

## Baseline Results
Current baseline configuration produces tonic firing at approximately:
* Firing rate: ~60 Hz
* ISI: ~15–20 ms
This serves as a reference operating point.

## Planned Updates
* Lower firing-rate regime (sparse, low-power operation)
* Event-driven synaptic pulse input
* Variability and robustness analysis

## Version History
* v1: Baseline tonic spiking (~60 Hz)
* v2 (planned): Sparse firing regime (~5–15 Hz)
* v3 (planned): Event-driven pulse input

## Why This Is Neuromorphic
This is not clocked digital logic. It is:
* Continuous-time
* Analog stateful dynamics
* Event-based (spike) communication
It represents a circuit-level abstraction of biological neuron behavior.

## Author
Ananya Parashar 

## Keywords
Neuromorphic Computing, Leaky Integrate-and-Fire, Event-Driven Circuits, NGSpice, Analog Neuron Models


# neuromorphic-lif
Circuit-level LIF neuron model in NGSpice with refractory reset for neuromorphic systems.

