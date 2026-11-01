# Intelligence Gathering, Red Teaming, and Adversarial Assessment of UAS
Adversarial assessment requires a defined scope, an authorized test environment, and procedures that make results reproducible and attributable. The assessment plan should identify the systems under evaluation, permitted activities, excluded assets, termination conditions, and responsible personnel. Document hardware and software tools, including their versions, configurations, and measurement limitations. An appropriate testbed separates assessment traffic from unrelated systems and supports synchronized logging across the aircraft, ground station, network, and measurement equipment. Computational requirements depend on the analysis task, including packet processing, signal inspection, log correlation, and offline data analysis.

## Practical Example: RF Disturbance and Packet-Replay Assessment
RF communication availability and packet freshness are distinct assessment concerns. Interference can degrade reception, while replay can cause previously generated packets to reappear in a communication stream. This exercise uses AeroResilience to inspect both mechanisms and document the evidence required to assess their effects separately.

### Practical Exercise 5.1: Distinguishing RF Availability Effects from Packet-Freshness Concerns
**Objective:** Inspect simultaneous modeled RF disturbance and synthetic packet replay, preserve evidence from both workflow branches, and identify the additional receiver-validation mechanism required to evaluate replay rejection.

**Worked situation:** Load `Jamming_Injected_Replay_Attack_2400MHz.json`. According to the supplied scenario description, the workflow combines pulsed and tone interference with a separate packet branch containing a packet source and a replay block. The replay configuration specifies a five-frame packet age and a probability parameter of $0.3$ during the configured exposure interval. Verify these settings and the parameter definitions in the loaded implementation before interpreting results.
Supporting scenarios are provided through the book’s repository:
- UAV CyberPhysical Security repository
- Practical procedure.

Preserve the original JSON configuration and record the software revision, exposure interval, model assumptions, and random seed where applicable. Trace the pulsed and tone interference components through the RF branch. Record their configured activation schedules and available communication-performance outputs. Inspect the separate packet branch, identifying the packet source, replay block, output connections, and available packet identifiers or sequence information. Confirm the configured five-frame age and probability parameter of $0.3$. Determine whether the probability applies per frame, per packet, or per eligible replay event, and document how frame age is defined.
Run the scenario over a defined interval and retain synchronized observations from both branches. Record original packet-generation events and replay events where the implementation exposes them.
Assess RF availability and packet freshness separately. Identify which replay-rejection claims require an additional receiver validator and cannot be concluded from the JSON workflow alone.
Illustrated workflow. The workflow image below provides the configuration view used to inspect the RF and packet-processing branches.
![AeroResilience RF and packet-replay assessment](chapters/Chapter_5/figures/Practical_p1_ch_5.jpg)
AeroResilience workflow view for simultaneous modeled RF disturbance and synthetic packet replay. The RF branch represents pulsed and tone interference, while the separate packet branch supports inspection of the packet source and replay configuration.

**Hint:** A packet may be received successfully yet be stale or duplicated. Conversely, RF impairment may cause a legitimate fresh packet to be lost. A communication-quality metric alone cannot distinguish these cases. A five-frame age is not a physical time delay until the frame duration and timing convention are established.

**Worked solution RF availability:** Create an RF assessment record containing the exposure interval, pulsed and tone settings, and reported link-performance indicators. Compare the disturbed case with a matched baseline where supported. Identify whether displayed PDR is an expected model output or a measured packet-count ratio, and document whether replay-generated packets influence any reported counters. Interpret degradation as a modeled availability effect without treating it as evidence of failed authentication.

**Worked solution packet freshness:** Create a separate event record associating each replayed packet with its original packet identifier, original generation frame, replay frame, and available payload or provenance reference. If the implementation exposes a packet-generation frame $k_{\mathrm{source}}$ and replay frame $k_{\mathrm{replay}}$, evaluate the configured age using

$$
a_{\mathrm{packet}}
= k_{\mathrm{replay}}-k_{\mathrm{source}}.
$$

Check the result against the implementation’s frame-index convention. Do not assume a replay event occurs exactly five frames after every source packet, because eligibility, scheduling, and probability semantics must also be established.
The probability parameter $0.3$ is a configuration value, not a guaranteed observed replay fraction. Record the actual number of eligible events and replay events if available, together with the random seed and observation interval. If these details are not exposed, identify the missing instrumentation rather than inventing event records. The output image below provides the output view used to compare RF observations and packet-event evidence.
![AeroResilience RF and packet-replay assessment](chapters/Chapter_5/figures/Practical_p2_ch_5.jpg)
AeroResilience output view supporting assessment of RF availability effects and synthetic packet-replay behavior. Conclusions require branch-specific evidence and aligned observation intervals; successful packet delivery alone does not establish freshness or authentication.
Additional receiver validation. The supplied file description does not implement an authentication defense. Replay rejection must therefore be evaluated through an additional receiver-validation mechanism. Define that mechanism separately, including how it handles packet identity, sequence numbers or counters, freshness windows, session state, and integrity or authentication checks where implemented. Authentication alone does not establish freshness unless the protocol also validates freshness-related information and maintains the required state.

Retain the validator’s accept/reject decision and reason for each evaluated packet. Distinguish accepted replayed packets, rejected replayed packets, accepted legitimate packets, incorrectly rejected legitimate packets, and packets not delivered to the validator. Do not count RF loss as a successful replay rejection. Any rejection-rate denominator must include only replay packets actually presented to the validator. If no validator is connected, report replay rejection as not evaluated.
Assessment report and evidence provenance. Organize the report into two linked evidence sets:

* **RF availability record:** Configuration reference, observation interval, RF outputs, baseline comparison, and model limitations.
* **Packet-freshness record:** Packet identifiers, source and replay timing, configured age, probability semantics, observed replay events, and validator results where available. For each evidence item, document its origin, collection method, software revision, time or frame reference, and any transformations applied. Separate configured behavior, observed simulation events, receiver-validation results, and interpretation. The report must not claim an implemented authentication defense or successful replay rejection without supporting receiver evidence.

Implementation checklist:
- [ ] Retain packet identifiers and original-generation references where available.
- [ ] Record replay timing and verify the five-frame age convention.
- [ ] Document the application rule for the probability parameter $0.3$.
- [ ] Inspect both the RF and packet branches and verify their output selection.
- [ ] Align observation intervals and preserve branch-specific records.
- [ ] Separate RF availability effects from packet-freshness concerns.
- [ ] Evaluate replay rejection only with an explicitly documented receiver validator.
- [ ] Document evidence provenance, missing instrumentation, and assessment limitations.

**Expected deliverable:** An assessment report distinguishing modeled RF availability effects from packet-freshness concerns, with traceable packet-event records and either documented receiver-validation results or an explicit statement that replay rejection was not evaluated.

## Practical Example: Evidence-Based Red and Purple Team Evaluation

References to named relations below refer to equations in the full chapter that were not included in the supplied excerpt. The numerical equations supplied with the exercise are retained. This exercise applies the RF-intelligence, red-team coverage, defensive-effectiveness, and reporting equations developed in this chapter. An initial red-team assessment identifies gaps; a purple-team cycle connects those gaps to blue-team improvements and independent retesting. Disturbance generation, alarm production, controller response, and mission outcome remain separate evidence items.

### Practical Exercise 5.2: Red-Team Assessment and Purple-Team Improvement
**Objective:** Evaluate a simulated UAV monitoring and response architecture using traceable RF observations, coverage matrices, response deadlines, recovery evidence, and before/after defensive measurements.
**Worked situation:** A ground-control emulator communicates with a UAV emulator during a virtual observation mission. The red team introduces scheduled RF disturbance and synthetic replay or unauthorized-command events through documented test interfaces. The blue team monitors RF features, packet-validation decisions, and emulator state. The purple team reviews evidence jointly, implements a bounded correction, and organizes a frozen retest. Use simulation or recorded laboratory data. A physical extension requires separately authorized, instrumented, contained testing. The exercise does not assume that an RF waveform causes packet loss or flight-mode changes unless the implementation explicitly connects and verifies those effects.

1. Define the campaign and roles. Instantiate the campaign tuple in the relation (red team tuple):
* $\mathcal{A}$: evaluate detection of disturbed communication and rejection of synthetic invalid commands.
* $\mathcal{S}$: RF receiver model, C2 receiver emulator, and their monitoring interfaces; include mission-state evidence only where implemented.
* $\mathcal{M}$: a virtual observation mission with documented essential communication and control requirements.
* $\mathcal{C}$: approved interfaces, finite exposure windows, evidence retention, prohibited actions, and explicit stop conditions.
* $\mathcal{D}$: an energy monitor, a feature/anomaly detector where implemented, packet-validation logs, and a response playbook.
* $\mathcal{G}$: emulator state bounds, operator override, test-stop authority, and RF containment for any separate hardware extension.
Assign a red-team operator, blue-team reviewer, purple-team coordinator, and evidence custodian. Identify who approves configuration changes and acceptance decisions. Freeze the initial detector settings before testing and record every subsequent modification.

2. Establish RF observations and their limits. Use the relation (rf received signal) to document the modeled emitters, channel coefficients, delays, offsets, and noise. Compute the window energy using the relation (rf energy detector) and apply the relation (energy decision rule) with a threshold fitted to separate baseline observations. For a normalized illustration, suppose $\lambda_E=1.5$ and one window has $E_k=2.1$. The energy detector reports signal presence. It does not identify an attack: the hypotheses in the relation (rf binary hypothesis) distinguish noise only from signal present. A legitimate active C2 link also contains a signal. Use a separate attack/anomaly rule and representative benign traffic when evaluating security alarms. Extract only the supported components of the relation (rf feature vector). For spectral features based on the relation (rf periodogram), state the FFT window, sample rate, frequency-bin interpretation, and normalization; the squared DFT magnitude is not automatically a calibrated power spectral density. If you use cyclic features, document the finite-data estimator supporting the relations (cyclic autocorrelation) and (spectral correlation density).
For the relation (rf anomaly score), estimate mission-phase means and covariances from development data only. Address singular covariance with stated regularization or a reduced feature set. Do not silently invert an ill-conditioned matrix. Report unavailable features explicitly. If using the relation (rf jamming indicator), declare the power representation and use linear SNR ratios for $SNR_k/SNR_{nom}$ rather than dividing dB values. Require positive, nonzero normalization denominators. For an illustrative normalized noise elevation of 2, SNR ratio 0.5, occupied-band ratio 0.4, and weights $(0.4,0.4,0.2)$,

$$
J_k^{RF}=0.4(2)+0.4(1-0.5)+0.2(0.4)=1.08.
$$

This is a heuristic index, not a probability and not necessarily bounded by one. Before applying the relation (rf intelligence risk) or the relation (blue team fusion score), define compatible component scales, score direction, missing-data handling, and weights. Do not count the same underlying observation repeatedly as independent corroboration.
4. Execute the red-team baseline. Define authorized technique columns as RF-disturbance emulation, synthetic replay testing, and unauthorized-command validation. Define two subsystem rows as the RF receiver model and C2 receiver emulator. For an illustrative executed-test matrix,

$$
\mathbf{E}=\begin{bmatrix}1&1&0\0&1&0\end{bmatrix},
\qquad
\mathbf{W}=\begin{bmatrix}3&2&1\2&3&1\end{bmatrix}.
$$

Here an RF-receiver replay test means assessing its observations during a packet-replay scenario, not claiming that the RF model validates packet freshness. Explain each test pairing and its expected evidence. Under a deliberately declared full six-pair catalog, the relations (red team coverage) and (weighted red team coverage) give

$$
C_{RT}=\frac{3}{6}=0.50,
\qquad C_{WRT}=\frac{3+2+3}{12}=0.667.
$$

If a pairing is genuinely inapplicable, declare that before testing, exclude it from the applicable denominator, and report the exclusion. Do not use an untested or incompatible pairing to imply supported capability. Test execution coverage is different from detection success and mitigation coverage.

3. Measure detection, response, and recovery. For each labeled event $q$, retain actual exposure onset, alarm time, effective mitigation onset, and sustained recovery acceptance. Do not substitute a timer label for observed onset or a response request for effective action.
In a synthetic event, let $t^{start}=100.0$ s, $t^{detect}=101.2$ s, and $t^{mitigate}=102.4$ s. Using the relations (red-team detection latency)–(red-team safety deadline),

$$
L^{detect}=1.2,\mathrm{s},\quad L^{resp}=1.2,\mathrm{s},
\quad L^{detect}+L^{resp}=2.4,\mathrm{s}.
$$

For a declared $T^{safe}=3.0$ s, this event meets the total-delay criterion. Report missed alarms and ineffective responses separately; do not give them zero latency or omit them from coverage counts. For the relation (red team detection effectiveness), normalize nonnegative weights to sum to one when claiming $D_q\in[0,1]$, define correctness and alert-quality rubrics, and explicitly treat no-alarm events. For the relation (red team recovery effectiveness), normalize applicable recovery weights and require controller or mission evidence for stabilization, geofence preservation, and landing/return behavior. An RF-only model cannot establish those outcomes; mark them not evaluated rather than inventing successful recovery.

4. Conduct the purple-team improvement cycle. Jointly inspect an initial finding, identify the failed mechanism, and implement a specific change such as a calibrated anomaly threshold, an actual replay validator, or a clearer operator alert and response procedure. Identify which software component changes and how you verify the behavior. Do not claim that modifying a label or adding a display scope implements a defense. Use a fixed four-scenario catalog with weights $(3,2,3,2)$. Suppose validated control coverage changes from $(1,0,0,1)$ to $(1,1,1,1)$. Using the relations (defense coverage score) and (defense coverage improvement),
   
$$
C_{def}^{pre}=\frac{5}{10}=0.50,\quad
C_{def}^{post}=1.00,\quad \Delta C_{def}=0.50.
$$

This is a 50-percentage-point increase for the declared catalog, not proof of complete UAV security. A proposed but untested control receives no validated-coverage credit. For a synthetic held-out comparison, suppose 20 independent labeled events yield 14 detected events before and 18 after improvement, while 100 defined benign observation units yield 8 and 3 false alarms. Then $P_d$ changes from 0.70 to 0.90 and $P_{fa}$ from 0.08 to 0.03. State decision units, denominators, event grouping, timing resolution, and uncertainty; do not equate event-level detection probabilities with frame-level rates without explanation. With mean detected-event delays of 1.2 and 0.5 s, $T_{safe}=3$ s, dimensionless resource overheads of 0.10 and 0.15, and coefficients $(\alpha,\beta,\gamma,\delta)=(0.5,0.2,0.2,0.1)$, the relation (blue team detection effectiveness) gives

$$
\begin{aligned}
E_{det}^{pre}&=0.5(0.70)-0.2(0.08)-0.2(1.2/3)-0.1(0.10)=0.244,\
E_{det}^{post}&=0.5(0.90)-0.2(0.03)-0.2(0.5/3)-0.1(0.15)\approx0.396.
\end{aligned}
$$

Define how you normalize and measure resource overhead. Report undetected events alongside these conditional mean delays to avoid implying that every event was detected. The composite score is an engineering index, not a calibrated risk probability.
Retest with frozen settings and matched conditions. Reserve independent runs for final verification after collaborative tuning; repeat across supported random seeds. Preserve degradation, false alarms, missed events, deadline violations, and unsuccessful fixes in the report.

6. Score findings and record lessons. Use the relation (red team scenario score) for scenario prioritization only after defining all component rubrics and score ranges. Do not substitute a scenario score for the finding-level risk in the relation (red team risk score): their variables and purposes differ, including the meaning of $D$. For finding-level severity, document cyber and physical components using the (cyber severity red-team) and (physical severity red-team) relations. Report any unobserved physical consequence as a hypothesis or unevaluated component. Keep weights, scales, and applicability fixed for before/after comparisons. Interpret $\Delta R$ in the relation (residual risk reduction) only within that declared scoring policy; a subtractive mitigation term can produce an index outside $[0,1]$ unless a range convention is explicitly defined. Create a finding record using the relation (red team finding tuple) and a corrective-action record using the relation (lesson learned tuple). Include the root cause, affected asset, evidence, mission consequence, corrective action, owner, verification test, and residual uncertainty. Use the (red-team detection coverage) and (red-team mitigation coverage) relations to report detected and effectively mitigated actions separately, retaining unsuccessful cases.

Implementation checklist:
- [ ] Define the campaign tuple, authorized scope, team roles, and stop authority.
- [ ] Distinguish RF presence detection, anomaly detection, and malicious-intent attribution.
- [ ] Record feature normalization, thresholds, weights, and missing-data rules.
- [ ] Separate test coverage, validated defense coverage, and successful mitigation.
- [ ] Verify alarm, effective response, and recovery with aligned evidence.
- [ ] Compare frozen pre- and post-configurations and report independent retest results.
- [ ] Measure execution costs and retain unsuccessful cases and uncertainty.
- [ ] Assign corrective-action owners and document residual-risk review.

**Expected deliverable:** A red-team campaign record, RF observation analysis, coverage matrices, event timeline, purple-team before/after comparison, and finding/lesson records supported by reproducible evidence. All numerical examples above are synthetic; replace them with actual observations before reporting experimental conclusions.
