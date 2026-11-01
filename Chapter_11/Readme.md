# Electronic Warfare Threats and Low-Probability-of-Intercept Communication Strategies

## RF Testbed Architectures and Experimental Evaluation of Communication Resilience
Converted from the supplied chapter text. Numerical examples are classroom calculations; the laboratory architecture is proposed. Named relations whose equations were outside the supplied excerpt refer to the full chapter. Figure paths are relative to the repository root and require the corresponding image files.
To evaluate communication resilience, set up a controlled testbed with communication endpoints, a configurable interference source, measurement tools, and synchronized logging. Depending on your experiment, use either conducted connections or a shielded radiated environment. Choose equipment based on signal characteristics, measurement bandwidth, timing needs, and the uncertainty of your evaluation. Use software to manage test conditions, record observations, and link performance measurements to each experiment setup. Key results to track are the packet-delivery ratio, latency, outage duration, recovery time, and mission-level effects. Make sure your system can handle real-time signal processing, data transfer, and storage needs.
Practical Example: Reproducible Interference Comparison and Laboratory Mapping
Reproducible interference assessment requires consistent timing, documented observation points, and a clear separation between simulated behavior and measured hardware results. This exercise compares a structured multi-tone exposure with a mixed RF disturbance and develops a conceptual laboratory architecture for subsequent validation.

### Practical Exercise 11.1: Multi-Tone and Mixed-Disturbance Comparison
Objective. Resolve a scenario-label inconsistency, compare two interference configurations under documented conditions, and map the simulation functions to a prospective controlled laboratory setup.
Worked situation. Load `Multi_Tone_Barrage_Attack_2400MHz.json` to examine a configured eight-tone interference model passing through RF-link, antenna, channel, and scope blocks. The supplied description identifies timer parameters with a start value of 5 and a duration of 75, even though a block label refers to frames 10–69. Use the timer parameters as the configuration timing reference, then verify their interpretation and actual generated activity in the implementation.
Compare this structured disturbance with the mixed barrage, noise, and tone configuration in `RF_DoS_Resilience_2400MHz.json`. Verify both loaded configurations rather than treating their filenames or block labels as complete technical specifications.
Supporting scenarios are provided through the book’s repository:
UAV CyberPhysical Security repository
Practical procedure.
Preserve both original JSON files and record repository revisions, simulator version, waveform parameters, channel settings, observation intervals, and random seeds where applicable.
Inspect the timer associated with the eight-tone model. Record the numeric start and duration, along with the inconsistent display label. Verify units, interval boundaries, frame origin, and any repetition behavior.
Trace each waveform through the RF-link, antenna, channel, and scope blocks. Identify where the system calculates or observes power and signal-quality indicators.
Establish and retain an interference-free baseline. Run both disturbance configurations with matched legitimate traffic, receiver settings, channel assumptions, and observation windows where supported.
Compare timing, spectral occupancy, received interference power, and available communication-performance outputs. State the power-normalization convention and any remaining differences between scenarios.
Develop a conceptual laboratory architecture, identify required calibration and instrumentation, and state which results remain simulation-only.
Timing inconsistency and resolution. The numeric start value 5 and duration 75 do not support a display label of frames 10–69. If the implementation uses zero-based indexing, frame units, and half-open intervals, the parameter-defined window is

$$
5\leq k<5+75=80,
$$

corresponding to frames 5–79. Verify that convention before assigning reference labels. Preserve the original file, record the discrepancy, and, if appropriate, correct the display label in a separately versioned working copy. Do not silently change the numeric timer parameters to match the label. Confirm actual activity using generated-waveform or event evidence, since a scheduled window and actual in-band exposure need not coincide.
Illustrated configuration. The configuration image below shows the eight-tone workflow, numeric timer settings, and the inconsistent label.
![Configuration and comparison view](chapters/Chapter_11/figures/Practical_p1_ch_11.jpg)
Proposed configuration view for the eight-tone interference scenario. Numeric timer parameters provide the configuration timing reference; document the conflicting block label and resolve it against implementation semantics and actual waveform activity.
Hint. Eight tones do not imply equal per-tone powers, equal spacing, or a particular total received power. Inspect the implemented model. Likewise, a mixed waveform may not match the linear sum suggested by its labels. Verify composition, normalization, timing, and receiver-band overlap before comparing disturbance severity.
Worked solution: reproducible simulation comparison. Create three assessment records: the interference-free baseline, the eight-tone exposure, and the mixed barrage/noise/tone exposure. Retain configuration identifiers and synchronized observation windows for each. Keep the legitimate signal, receiver bandwidth, antenna gains, propagation assumptions, and traffic definition consistent where possible. Reuse channel realizations or seeds for paired comparisons where supported; otherwise repeat trials and report variability.
Choose and justify a comparison basis, such as equal total in-band interference power at a defined receiver reference point. Alternatively, compare the original configurations as supplied, explicitly reporting their different powers and other settings. Do not claim a waveform-only comparison when received power, exposure duration, or channel conditions also differ. Specify whether power is averaged over active intervals or the full observation period, and whether peaks are recorded separately.
Measure spectral occupancy and time-domain activity at documented observation points. Compare available expected PDR, SNR, SINR, and $J/S$ with the baseline, identifying the definitions and integration bandwidths used. Distinguish model-estimated PDR from packet-count measurements. A relative spectrum plot is not automatically a calibrated power spectral density or an absolute dBm measurement.
Interpret any degradation only within the tested model and configuration. The filename `RF_DoS_Resilience_2400MHz.json` does not by itself establish denial of service, demonstrated resilience, or an implemented countermeasure. Such claims require defined service criteria and supporting observations.
Measurement locations. For every recorded quantity, identify whether it is observed at a waveform-generator output, after antenna/channel transformations, at the receiver input, or after receiver processing. Retain the relevant bandwidth, averaging interval, selected branch, and frame index or timestamp. Keep configured reference powers separate from calculated received powers and independently calibrated hardware measurements. Verify that scopes being compared refer to equivalent points in the two workflows.
Illustrated observations and laboratory mapping. The comparison image below supports the baseline comparison and prospective mapping with laboratory measurements.
![Configuration and comparison view](chapters/Chapter_11/figures/Practical_p2_ch_11.jpg)
Proposed comparison view for structured multi-tone and mixed RF disturbance observations. A conceptual laboratory mapping identifies measurement reference points; separately acquired and calibrated hardware evidence must support experimental performance.
Worked solution: conceptual laboratory architecture. Map the preserved scenario configuration to host-based scheduling and signal-processing software. A compatible SDR or signal generator represents the disturbance source, while a separate communication transmitter and receiver provide the legitimate packet link. A characterized conducted combining and attenuation network, or a controlled shielded setup, represents the RF coupling path. Use measurement instruments and receiver logs to observe signal conditions and valid packet delivery at explicitly identified reference points.
Select SDR frequency coverage, instantaneous bandwidth, sample rate, dynamic range, and timing capabilities from the actual waveform and receiver requirements. Establish a calibration between digital amplitude, hardware gain settings, and RF output power. Account for cable, combining, attenuation, and channel-emulation losses, and observe instrument input limits. If antennas are included in a shielded radiated setup, document their gains, orientation, polarization, placement, and coupling assumptions; do not apply antenna terms again in a conducted configuration that already uses measured transfer loss.
The architecture is a proposal until it is implemented and verified. Separate simulation evidence, calibration records, hardware observations, and interpretations in the report. Do not transfer expected packet-delivery values from the simulation into the experimental-results table. Record timing alignment and any mismatches between programmed exposure and measured RF activity.

Implementation checklist.
- [ ] Resolve label–parameter inconsistencies without overwriting the original evidence.
- [ ] Verify timer units, boundaries, repetition, and actual exposure activity.
- [ ] Document waveform composition, normalization, and receiver-band overlap.
- [ ] Identify measurement locations, bandwidths, and power reference points.
- [ ] Retain interference-free baseline observations and matched comparison records.
- [ ] Distinguish simulation outputs from calibrated hardware results.
- [ ] Record software revisions, seeds, calibration requirements, and remaining limitations.

**Expected deliverable:** A reproducible simulation comparison, a documented resolution of the timer-label discrepancy, retained baseline observations, and a conceptual laboratory architecture identifying RF components, calibration needs, and measurement locations. Experimental claims remain conditional on separately collected hardware evidence.

### Practical Exercises: Anti-Jamming Link Adaptation and Mission Continuity
The following exercises apply the chapter’s link, channel-selection, detection, and multi-bearer equations. They compare controlled receiver-side responses under synthetic interference and retain evidence of unsuccessful outcomes. Numerical results are classroom calculations, not hardware measurements or operational guarantees.

**Practical Exercise 11.2:** Link Budget, Processing Gain, and Adaptive Channel Selection
**Objective:** Calculate interference-limited link quality, evaluate a processing-gain approximation, and select an eligible channel while accounting for switching cost and packet performance.
Worked situation. A simulated UAV receiver has desired received power $S=-70$ dBm, total in-band interference $J=-75$ dBm, and receiver noise $N=-100$ dBm over $B=125$ kHz. Powers refer to the same receiver input and bandwidth. These received powers already incorporate path loss, antenna gains, and fading; do not apply them again.
Use the receiver SINR relation, or its aggregate form in the aggregate SINR relation, to compute SINR. The declared classroom degradation thresholds are $\gamma_{\min}=6$ dB and $\mathrm{PER}_{\max}=0.10$. These values are exercise requirements, not universal UAV thresholds.
Tasks.
Calculate the SINR and the idealized rate bound using the receiver SINR and idealized rate-bound relations. Apply the link-degradation criterion relation.
For a separate spread-spectrum configuration with $B_s=2$ MHz and $R_b=20$ kbit/s, calculate the processing gain and the approximate post-processing $J/S$ using the processing-gain and post-processing interference-ratio relations.
Form the eligible channel set using the eligible-channel set relation and rank only eligible channels with the channel utility and best-channel selection relations.
Specify how a receiver implementation would verify improved packet delivery and measure transition costs.
Worked solution: SINR and rate. Convert dBm powers to linear units before adding interference and noise:

$$
\gamma=\frac{10^{-70/10}}{10^{-75/10}+10^{-100/10}}
\approx3.15,\qquad \gamma_{dB}\approx4.99,\mathrm{dB}.
$$

The common linear power unit here is milliwatts. the idealized rate bound relation gives

$$
R=125{,}000\log_2(1+3.15)\approx257,\mathrm{kbit/s}.
$$

This is an idealized information-rate bound under the assumed channel, not measured useful throughput. The link violates the 6 dB SINR criterion even before a PER estimate is available. Do not infer PER from this bound without specifying modulation, coding, packet length, and an error model; the PDR-model relation requires those definitions.
Worked solution: processing gain. The separate spread configuration yields

$$
G_p=\frac{2\times10^6}{20\times10^3}=100,
\qquad G_{p,dB}=20,\mathrm{dB}.
$$

For the illustrative input $J/S=-5$ dB, the post-processing interference-ratio relation predicts about $-25$ dB after processing under its simplifying assumptions. This does not automatically establish a 20 dB improvement in decoding SINR or PER. The spread receiver’s noise bandwidth, synchronization, waveform processing, and jammer overlap require a separate model. FHSS performance also depends on hop occupancy, dwell time, and coordination; do not treat this approximation as a guaranteed hopping benefit.
For coding analysis, use the spectral efficiency relation; under the chapter's convention, BPSK with $R_c=1/2$ gives $\zeta=0.5$ bits/s/Hz. The modulation and coding selection relation still requires an applicable error model. The bit-energy ratio $\gamma_b$ in the BPSK error-approximation relation is not automatically identical to the bandwidth-integrated SINR calculated above, and the BPSK approximation alone does not establish coded-packet performance.
Worked solution: channel selection. Use the following hypothetical receiver estimates. PER values are separately supplied, not derived from the previous SINR calculation.
Channel	SINR (dB)	PER	$\widehat I_k$	$C_k$
1 (current)	5	0.20	0.80	0.00
2	9	0.03	0.20	0.20
3	12	0.15	0.10	0.10
4	8	0.05	0.25	0.40
The eligible-channel set relation includes only channels 2 and 4. Define the SINR contribution in the channel utility relation using the declared normalized score $q_k=\min(\gamma_{k,dB}/10,1)$ for these nonnegative illustrative estimates, rather than mixing unscaled SINR with other normalized terms. With $(\alpha,\beta,\eta,\rho)=(0.4,0.4,0.1,0.1)$,

$$
\begin{aligned}
U_2&=0.4(0.9)+0.4(0.97)-0.1(0.2)-0.1(0.2)=0.708,\
U_4&=0.4(0.8)+0.4(0.95)-0.1(0.25)-0.1(0.4)=0.635.
\end{aligned}
$$

Thus the best-channel selection relation selects channel 2. Channel 3’s high SINR does not override its PER violation. If no eligible channel remains, report that result and invoke the predefined degraded-service policy rather than silently selecting a failed candidate.
Verification checklist:

- [ ] Preserve receiver power reference planes and use compatible bandwidths.
- [ ] State processing-gain, waveform, synchronization, and error-model assumptions.
- [ ] Normalize utility inputs and exclude ineligible or stale channel observations.
- [ ] Verify an actual coordinated channel transition, not only output selection.
- [ ] Measure outage during switching, useful delivery, latency, and processing cost.
- [ ] Retain baseline, improved, and unsuccessful cases under matched conditions.

**Expected deliverable:** A worked link calculation, qualified processing-gain estimate, eligible-channel ranking, and verification plan distinguishing predicted rates from measured packet service.

### Practical Exercise 11.3: Cumulative Detection, Multi-Path Recovery, and DTN Payload Buffering
Objective. Detect a sustained disturbance using joint power/packet evidence and assess a layered response combining alternate paths, bounded C2 service, and buffering of non-real-time payload data.
Worked situation. A monitoring system observes five consecutive evaluation windows. A contact change may reflect interference, fading, or congestion, so maintain benign comparison cases. Use the normalized idle-power deviations below and measured PDR estimates over explicitly defined packet windows:
Window $t$	1	2	3	4	5
Normalized power deviation	0.0	0.2	1.2	2.0	2.5
$\widehat{\mathrm{PDR}}$	0.98	0.95	0.80	0.50	0.40
Use $\lambda=2$ in the joint degradation indicator relation, initial state $S(0)=0$, drift $\nu=0.5$, and alarm threshold $h=4$ in the cumulative detector relation. The alarm rule is strictly $S(t)>h$.
Tasks.
Compute the combined indicator and cumulative score and identify the alarm window.
Compare two bearer choices using the bearer cost relation, with declared normalized costs and service constraints.
Calculate redundant path reliability using the multi-link availability and redundant-path reliability relations; identify common-cause dependencies.
Size payload buffering for a finite outage and define C2 deadlines separately.
Specify recovery acceptance, resource measurements, and accountability evidence using the mission utility and resilience metric relations as reporting frameworks.
Worked solution: detection. Using the joint degradation indicator relation,

$$
z(t)=[0.04,;0.30,;1.60,;3.00,;3.70].
$$

The cumulative detector relation then gives

$$
S(t)=[0,;0,;1.10,;3.60,;6.80].
$$

The first threshold crossing is window 5. To report detection delay, retain the separately labeled disturbance onset, window duration, overlap, and decision-availability time. These five inputs alone do not establish a delay in seconds or a detection probability. The combined indicator shows degradation, not proof of malicious intent. Calibrate false alarms with held-out benign records; a CUSUM construction alone does not guarantee robustness to fading or congestion.
Worked solution: bearer choice. For a normalized illustrative cost in the bearer cost relation, use weights $(\alpha,\beta,\chi,\delta,\rho)=(0.4,0.2,0.2,0.1,0.1)$ and the following data:
Bearer	$P_{out}$	$T$	$J$	$E$	$D$
A	0.30	0.20	0.80	0.20	0.10
B	0.05	0.40	0.20	0.40	0.80
The source reports costs of $0.33$ for A and $0.10$ for B, favoring B. The referenced bearer-cost equation is not included in the supplied excerpt, so check its sign convention and these costs against the full chapter before reuse. First verify the actual latency, authentication, freshness, and capacity requirements; normalized cost minimization cannot authorize a bearer that violates a mandatory C2 deadline. Declare normalizers and bounds before evaluation and confirm that bearer switching is implemented and coordinated.
Worked solution: redundant reliability. Suppose path A has two hop-delivery probabilities 0.90 and 0.95, and path B has 0.80 and 0.90. Under the within-path independence approximation, their path reliabilities are

$$
R_A=0.90(0.95)=0.855,\qquad R_B=0.80(0.90)=0.72.
$$

If the two complete paths are also independent, the redundant-path reliability relation gives

$$
R_{e2e}=1-(1-0.855)(1-0.72)=0.9594.
$$

This calculation concerns redundant delivery of the same packet, not selection of a single path. State duplicate suppression, offered load, and any delivery deadline. The multi-link availability relation uses a similar product for link availability, but link availability and per-packet timely delivery are not interchangeable quantities. Shared receivers, gateways, power supplies, weather, or interferers can invalidate independence and overstate the improvement. Use correlated observations or an explicit joint model for verification.
Worked solution: buffering and recovery. For a non-real-time payload source producing 80 kbit/s during a 30 s outage, the minimum payload-only storage is

$$
Q_{min}=80{,}000(30)=2.4\times10^6,\mathrm{bits}
=300{,}000,\mathrm{bytes}.
$$

A nominal 512,000-byte buffer exceeds this payload count but still needs to account for metadata, security overhead, existing occupancy, expiry policy, and retransmissions. After contact returns, the available service rate must exceed continuing payload arrivals to drain the backlog. This is a sizing calculation, not proof that a DTN implementation is present.
Do not buffer time-critical C2 indefinitely. Define command age limits, prioritize service, and approve a response when no timely trusted link exists. Verify recovery only after the required delivery, latency, and freshness criteria hold for a declared interval. Separate link restoration, controller state, and operator approval.
When applying the mission utility relation, define normalizers for rate, latency, power, and security score, and keep electrical energy distinct from RF transmit power. For the resilience metric relation, normalize recovery time or give its weight appropriate units; record the basis for any unsafe-state probability. A favorable aggregate must not compensate for violation of an essential safety criterion.
Verification checklist:
- [ ] Document packet-count windows, idle-power normalization, and alarm parameters.
- [ ] Compare benign and disturbed observations with independent labels.
- [ ] Verify bearer eligibility, transition timing, and trustworthy C2 delivery.
- [ ] Test common-cause failures rather than assuming independent redundant paths.
- [ ] Measure payload buffer occupancy, expiry, backlog draining, and overhead.
- [ ] Retain recovery evidence, computational costs, operator decisions, and failed cases.

**Expected deliverable:** A cumulative-detector calculation, a constrained bearer-selection record, a qualified path-reliability estimate, and a DTN/C2 recovery procedure supported by repeatable observations and assigned review responsibilities.
