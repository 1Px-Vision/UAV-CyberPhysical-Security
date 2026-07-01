#Drone Attack and Countermeasure Educational Simulator

## Short Description

The **Drone Attack and Countermeasure Educational Simulator** is an interactive Dash-based dashboard for education, book demonstrations, and defensive UAV security analysis. It simulates how drone communication and navigation can be affected by RF jamming, GPS spoofing, failsafe-triggering attacks, and degraded link conditions.

The simulator includes tabbed controls for mission setup, RF-link parameters, antenna gain, attack distance, jamming waveforms, GPS spoofing bias, navigation fusion, and countermeasure selection. Real-time scopes show waveform behavior, spectrum activity, waterfall view, link budget, packet delivery, navigation error, and mission safety state.

This project is a **safe abstract baseband-only simulator**. It does not transmit RF energy, control real drones, provide hardware instructions, or provide operational attack guidance. Its purpose is to support classroom learning, book figures, and defensive analysis. The uploaded simulator states that all signals are normalized numerical waveforms and that the tool is intended for education, book illustrations, and defensive analysis only. :contentReference[oaicite:0]{index=0}

## Simulated Attack Types

The simulator supports the following educational attack/anomaly models:

- No attack / nominal link
- Noise jamming
- Tone jamming
- Sweep jamming
- Barrage jamming
- Pulsed jamming
- Continuous jamming
- Intermittent jamming
- Reactive jamming
- Adaptive jamming
- Failsafe-triggering attack
- GPS spoofing

## Simulated Countermeasures

The simulator includes the following defensive countermeasure models:

- Spectral notch filtering
- Hopping / channel diversity
- GPS, VIO, and TDoA sensor-fusion gating
- Adaptive link manager and safe mode
- Combined defense stack

## Main Control Parameters

Users can adjust:

- Carrier frequency
- Signal bandwidth
- Legitimate transmitter power
- Attack-source power
- TX, RX, and attack antenna gain
- Drone link distance
- Attack-source distance
- Path-loss exponent
- Receiver noise figure
- J/S ratio trim
- Jammer center offset
- Sweep or barrage span
- Pulse/intermittent/reactive duty cycle
- GPS spoofing maximum bias
- Simulation update interval

## Requirements

- Python 3.10 or newer
- pip
- A modern web browser

Python packages:

```bash
dash
plotly
numpy
