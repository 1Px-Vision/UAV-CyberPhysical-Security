# Introduction, Related Work, and Technical Background

# AeroResilience

AeroResilience is a visual, block-based framework proposed for teaching and evaluating UAV communication resilience. Users configure interference models, timing schedules, RF links, antenna gains, and channel effects through interconnected blocks and reusable JSON scenarios. Power, waveform, spectrum, and metrics scopes let users inspect simulated communication conditions.

A proposed hardware interface would extend this workflow to controlled laboratory evaluation using compatible software-defined radio (SDR) platforms, GNU Radio, and measurement instruments, linking simulation-based exercises with experimental observations.

[Figure 1](#figure-1-connected-block-architecture) illustrates the connected block architecture and live link-budget display. [Figure 2](#figure-2-waveform-and-spectrum-scopes) shows the corresponding waveform and spectrum views used for qualitative inspection.

## Practical Exercise 1.1: Mapping Simulation Blocks to Hardware and Software

### Objective

Identify the hardware and software functions required to translate a simulated UAV communication-resilience scenario into a prospective laboratory architecture.

### Scenario and supporting material

Load `D2D_Narrow_2400MHz.json` in AeroResilience to examine a narrowband-interference scenario in a simulated 2.4 GHz communication environment.

The supporting JSON scenario is provided through the book's repository:

[UAV Cyber-Physical Security](https://github.com/1Px-Vision/UAV-CyberPhysical-Security)

### Practical task

1. Trace the connected sequence from the spot-interference model through the timer, RF-link, antenna-gain, and channel-effect blocks to the output power scope.
2. Identify configured inputs, including frequency, bandwidth, power settings, timing, antenna gains, and channel assumptions, where available.
3. Inspect the waveform, spectrum, and scenario metrics. Record the available SNR, SINR, interference-to-signal ratio ($J/S$), and expected packet delivery ratio (PDR).
4. Compare a baseline and an interference case while holding the remaining configuration fixed. Record any countermeasure settings used.
5. Annotate each block with its prospective hardware or software counterpart.

### Figure 1: Connected block architecture

![AeroResilience connected block architecture and live link-budget display](chapters/Chapter_1/figures/Practical_p1_ch_1.jpg)

*AeroResilience connected block architecture and live link-budget display for a simulated 2.4 GHz communication-resilience exercise. The processing chain includes the spot-interference model, timer, RF-link parameters, antenna gains, channel effects, and output power scope. The floating metrics panel and live link-budget display show different frame indices and therefore represent separate simulation states.*

### Hint

For each block, ask whether it handles configuration, scheduling, signal generation, propagation modeling, reception, or measurement. Identify where a simulated quantity would require calibration before comparison with a physical measurement.

### Worked situation and solution

The blocks can be mapped to the following prospective hardware and software functions:

| Simulation component | Responsibility | Prospective hardware or software counterpart |
|---|---|---|
| Spot-interference model | Signal generation | Software-defined signal source with a compatible SDR or RF signal generator |
| Timer | Scheduling and synchronization | Software scheduler and synchronization functions |
| RF-link block | Communication-link configuration | Transmitter and receiver parameters |
| Antenna-gain block | Antenna characterization | Characterized antennas or equivalent calibrated gains and losses |
| Channel-effect blocks | Propagation modeling | Controlled propagation conditions or calibrated attenuation in a conducted setup |
| Output scopes | Signal inspection and measurement | Analysis software and measurement instruments |
| Metrics panel | Performance calculations and logging | Software calculations and experiment logs |

GNU Radio and device drivers would support signal processing and SDR control in the proposed laboratory interface.

The resulting annotated architecture connects signal-generation software and RF equipment to the communication receiver and measurement path. A laboratory implementation requires a documented, calibrated, conducted or shielded arrangement. Simulator power settings must be related to defined RF reference points before they can be compared with physical measurements.

### Interpretation of results

**Expected PDR is a model output.** Experimental PDR requires transmitted-packet and valid-received-packet counts over a defined observation interval:

$$
\mathrm{PDR}_{\mathrm{measured}}
=
\frac{N_{\mathrm{valid,received}}}
     {N_{\mathrm{transmitted}}}
\times 100\%.
$$

State the packet-validity criteria and counting interval.

Treat normalized waveform amplitudes and relative spectral magnitudes as simulation representations, not calibrated RF power measurements. Compare scope and metrics values from the same frame or observation interval.

In [Figure 1](#figure-1-connected-block-architecture), the floating metrics panel shows **frame 0**, while the live link-budget display shows **frame 138**. Do not interpret these values as a synchronized record.

### Figure 2: Waveform and spectrum scopes

![AeroResilience simulated waveform and spectrum scopes](chapters/Chapter_1/figures/Practical_p2_ch_1.jpg)

*AeroResilience simulated waveform and spectrum scopes. The upper view compares the legitimate link signal, simulated interference component, raw received signal, and modeled response after countermeasure processing. The lower view compares their spectral representations. Waveform amplitudes are normalized and spectral magnitudes are relative; these plots do not constitute calibrated RF measurements.*

### Implementation checklist

- [ ] Identify each block's responsibility.
- [ ] Separate configured parameters, calculated outputs, and measured quantities.
- [ ] Map each function to its prospective hardware or software counterpart.
- [ ] Document units, bandwidths, power reference points, and channel assumptions.
- [ ] Record baseline and interference-case results using consistent observation intervals.
- [ ] Document calibration requirements and implementation limitations.
- [ ] Preserve the JSON configuration, software revision, repository commit identifier, and random seed where applicable.

### Expected deliverable

Submit an annotated hardware–software architecture, a table distinguishing configured parameters from calculated outputs, and a brief comparison of baseline and interference-case behavior.
