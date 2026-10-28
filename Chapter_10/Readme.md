# UAS System Deployment, Mission Assurance, and Information Dominance

## Mission Assurance, Component Reliability, and GNSS Spoofing Assessment
**Practical Example:** Disturbance Response and Recovery Assurance
Mission assurance requires explicit criteria for recognizing degraded communication, escalating a disturbance, accepting recovery, and reviewing the event. This exercise combines two AeroResilience teaching activities while distinguishing simulated link behavior from controller state transitions that require separate implementation or telemetry evidence.

**Practical Exercise 10.1:** Disturbance Response, Recovery Acceptance, and Operator Responsibilities
Objective. Assess communication performance before, during, and after a scheduled disturbance, and develop a mission-assurance procedure supported by measurable thresholds, transition evidence, recovery verification, and assigned responsibilities.
Worked situation. Use the teaching activities associated with `Failsafe_2400MHz.json` and `D2D_Recovery_2400MHz.json`. Treat them as complementary evaluations unless you have developed and verified an explicit combined implementation. According to the supplied description, the recovery workflow schedules noise exposure from frame 20 for 40 frames within a 90-frame period. Inspect the timer implementation, frame origin, interval boundaries, and repetition behavior before assigning labels.
The failsafe waveform label does not demonstrate an actual flight-controller mode transition. Establishing such a transition requires a separately implemented controller model or recorded flight-controller telemetry. Preserve that distinction throughout the report.

Supporting scenarios are provided through the book’s repository:
https://github.com/1Px-Vision/UAV-CyberPhysical-Security

### Practical procedure.
Preserve both JSON files and record the simulator revision, timer settings, frame timing, relevant model parameters, and random seeds where applicable.
Define disturbance-detection, escalation, and recovery-acceptance criteria before execution. Specify the evaluated statistic, threshold, persistence interval, and how you handle missing observations.
Verify the scheduled noise interval against actual waveform activity at the evaluated receiver input. Preserve scheduled and actual labels separately.
Compare equivalent link-performance records before, during, and after exposure. Retain the frame index or timestamp, selected branch, available expected PDR, SNR, SINR, and missing-data indicators.
Use the failsafe activity to identify the controller and telemetry evidence required to verify a mission-state transition. Do not infer controller behavior from a waveform name.
Apply the recovery criterion, document any continued degradation or recurrence, and prepare the mission-assurance procedure and post-event report.
Exposure windows. For illustration, if the implementation uses zero-based indexing and a half-open interval, the first cycle is partitioned as follows:

<div class="center">
Assessment phase	Frame indices
Before exposure	$0\leq k<20$
Scheduled exposure	$20\leq k<60$
After scheduled exposure	$60\leq k<90$
</div>
  
Verify this convention against the loaded timer rather than assuming it. A 90-frame period may imply repetition; it does not necessarily define the total run length. You cannot convert frame counts to seconds without a documented frame duration. An interval labeled “after exposure” is a recovery-observation window, not proof that recovery has occurred.
Illustrated disturbance schedule. Figure 1 shows the recovery scenario and its exposure timing.
![Figure 1: Disturbance schedule](chapters/Chapter_10/figures/Practical_p1_ch_10.jpg)

Proposed AeroResilience recovery-scenario view showing scheduled noise exposure and the link-performance observation path. Before-, during-, and after-exposure labels require verification of timer boundaries and actual interference activity.
Hint. Distinguish between disturbance onset, detector alarm, escalation decision, controller command, confirmed controller state, exposure cessation, and accepted recovery. These events may occur at different times. A command log alone is not confirmation that the flight controller has entered the requested mode.
Worked solution: mission-assurance procedure.
Baseline and readiness. Establish the pre-exposure performance record and verify that required observations are valid. Record the mission requirement and any conditions that prevent a meaningful assessment.
Disturbance detection. Apply the declared detection rule to the observed outputs. Retain the first qualifying alarm and its supporting observation window. Noise being scheduled is reference evidence, not a detector decision itself.
Escalation. Specify when degradation persists long enough or becomes severe enough to require operator review or a predefined controller response. Assign the responsible operator and document the applicable procedure and approval boundary.
Transition confirmation. Where a controller is evaluated, record the command or trigger, acknowledgment where available, and independently reported controller state. Otherwise, mark the flight-mode transition as not evaluated.
Recovery verification. After actual exposure ends, require a valid, sufficiently sustained performance interval that meets the predefined recovery criterion. Check controller state and other mission prerequisites separately where those functions are in scope.
Post-event review. Reconstruct the timeline, document acceptance or non-acceptance, identify unresolved evidence gaps, and assign corrective actions and review responsibilities.
Worked solution: recovery acceptance. A possible communication criterion requires the selected performance statistic to remain above a justified threshold $P_{\mathrm{rec}}$ for $M$ consecutive valid observations. This is a proposed rule, not a function claimed for the JSON. Define the observation cadence, whether windows overlap, the minimum valid-data coverage, and how missing data resets or suspends the assessment. If expected PDR is used, identify recovery as simulation-based rather than experimentally demonstrated packet delivery.
Define the recovery time using actual exposure cessation $t_{\mathrm{off}}$ and the instant when the full recovery rule is first satisfied:

$$
T_{\mathrm{rec}}=t_{\mathrm{accepted}}-t_{\mathrm{off}}.
$$

State whether timing is expressed in frames or seconds. If the criterion was already satisfied at exposure cessation and remains satisfied for the required confirmation interval, record that behavior without claiming an observed outage. If recovery is not accepted before the observation ends or the next exposure begins, report it as not demonstrated within the available window rather than assigning an arbitrary recovery time.
Compare pre-exposure, exposure, and post-exposure statistics, and retain transient effects. A short improvement after cessation is insufficient when the declared rule requires persistence. Link recovery alone does not authorize mission resumption; the responsible authority must review all applicable mission conditions.
Illustrated recovery evidence. Figure 2 shows the observations used to verify recovery and, where available, distinguishes them from controller-transition evidence.

![Figure 2: Recovery evidence](chapters/Chapter_10/figures/Practical_p2_ch_10.jpg)

Proposed observation view for disturbance and recovery assessment. Link-performance records support communication-recovery analysis; a failsafe label alone does not establish a flight-controller transition, which requires separate controller-model or telemetry evidence.
Evidence and operator responsibilities. Retain configuration references, actual exposure activity, alarms, escalation decisions, controller evidence where available, recovery observations, and acceptance records using aligned time or frame references. Identify the scenario operator, monitoring reviewer, controller or telemetry custodian, and authority responsible for recovery acceptance or mission resumption. Document who preserves evidence, reviews anomalies, and closes corrective actions. If a required role or evidence source is absent, record the resulting assessment limitation.
Implementation checklist:

- [ ] Define acceptance thresholds, observation windows, and persistence criteria before execution.
- [ ] Verify exposure boundaries, repetition, and actual waveform activity.
- [ ] Record distinct detection, escalation, command, and confirmed-state events.
- [ ] Require separate evidence for flight-controller mode transitions.
- [ ] Verify sustained recovery and retain non-recovery or recurrence cases.
- [ ] Distinguish modeled link recovery from mission-resumption approval.
- [ ] Document operator responsibilities, evidence provenance, and corrective-action ownership.
- [ ] Expected deliverable. A mission-assurance procedure defining disturbance detection, escalation, recovery acceptance, and post-event review, supported by before-, during-, and after-exposure records. Any flight-controller transition must be supported by separate controller-model or telemetry evidence, or explicitly reported as not evaluated.

### Practical Example: Component Failures and Supply-Chain Investigation

Reliability monitoring can reveal a change in component performance that warrants a supply-chain investigation. Increased failures do not by themselves establish deliberate compromise. This exercise combines synthetic failure statistics with procurement, manufacturing, maintenance, and configuration evidence to distinguish an observed reliability problem from a supported supply-chain attack hypothesis.
**Practical Exercise 10.2:** Rising Component Failure Rates and Suspected Supply-Chain Compromise
Objective. Analyze a change in drone-component failure rates, estimate a reliability indicator under explicit assumptions, and design an evidence-based investigation of a potentially compromised replacement-parts batch.
Terminology. This exercise interprets “MTF” as mean time to failure, conventionally abbreviated MTTF. Increasing failure frequency generally corresponds to decreasing MTTF under comparable conditions. Distinguish MTTF for a non-repaired component lifetime from mean time between failures (MTBF) for a repairable system. Do not label an increased failure rate as an increased MTTF.
Worked situation. An operator introduces replacement electronic speed controller (ESC) modules from a new supply batch. During subsequent operations, a larger proportion of module failures is observed. Students must assess the statistical change and determine what evidence would distinguish ordinary quality defects, altered operating conditions, counterfeit parts, unauthorized firmware, or deliberate tampering.
Use the synthetic dataset below. Exposure is the sum of observed component operating hours at risk. Count each module’s first defined failure only; stop its exposure at failure or at the end of observation. Treat each replacement as a new identified component, not a continuation of the failed component’s lifetime. Assume the stated dataset is complete for the classroom calculation; assess that assumption during the investigation.
Synthetic component statistics.

<div class="center">
ESC cohort	Hours at risk	First failures	Failures/1,000 h
Reference batch	20,000	5	0.25
New batch, subgroup A	5,000	5	1.00
New batch, subgroup B	5,000	10	2.00
New batch, subgroup C	5,000	15	3.00
New batch, pooled	15,000	30	2.00
</div>

The pooled row summarizes A–C; it is not an additional independent cohort. The ordered subgroup rates illustrate an increasing descriptive pattern, not proof of a temporal trend or a single component’s increasing hazard. Establish whether subgroups correspond to time, use conditions, or sampling differences before making that claim.
Practical procedure.
Define failure consistently, including diagnostic criteria, component identity, and how you treat suspected versus confirmed failures.
Compute failures per operating hour for each cohort and the pooled new batch. Compare matched component types and record exposure coverage.
Estimate the inverse failure-rate reliability indicator under the stated constant-hazard model. Explain why it is not a directly observed average lifetime.
Quantify uncertainty and compare operating conditions, component age, mission load, temperature, maintenance, and firmware versions before interpreting batch differences.
Build competing explanations and identify provenance, physical-inspection, and firmware evidence needed to evaluate each one.
Propose containment, verification, and return-to-service criteria with assigned responsibilities and retained evidence.
Hint. Raw failure counts are not comparable when exposure differs. A lot with more failures may simply have more operating hours. Likewise, premature observation termination, missing failed parts, or inconsistent failure definitions can bias the estimated rates. Track components that have not failed as censored observations rather than discarding them.
Worked solution: failure-rate estimates. For $r$ observed first failures over total exposure $T$, estimate an exposure-normalized rate by

$$
\widehat{\lambda}=\frac{r}{T}.
$$
For the reference and pooled new batch,
$$

\widehat{\lambda}{\mathrm{ref}}=\frac{5}{20{,}000}
=0.00025,\mathrm{h}^{-1},
\qquad
\widehat{\lambda}{\mathrm{new}}=\frac{30}{15{,}000}
=0.002,\mathrm{h}^{-1}.
$$

The descriptive rate ratio is

$$
\widehat{\mathrm{RR}}
=\frac{\widehat{\lambda}{\mathrm{new}}}
{\widehat{\lambda}{\mathrm{ref}}}=8.
$$

This is an eightfold estimated exposure-normalized failure rate in the supplied synthetic data. It is not an eightfold probability of failure for every mission and does not establish the cause of the failures.
Worked solution: MTTF interpretation. Under an independent exponential-lifetime model with a constant hazard and non-informative censoring, the maximum-likelihood estimate is

$$
\widehat{\mathrm{MTTF}}
=\frac{1}{\widehat{\lambda}}=\frac{T}{r}.
$$

Thus the reference estimate is 4,000 h and the pooled new-batch estimate is 500 h. The corresponding subgroup estimates for A, B, and C are 1,000, 500, and approximately 333 h. These values demonstrate decreasing model-based MTTF as estimated failure rate increases.
Do not interpret these estimates as directly measured mean lifetimes, guaranteed endurance, or verified wear-out behavior. Pooling heterogeneous subgroups imposes an additional common-rate approximation. If hazard varies with component age or conditions, retain individual lifetime and censoring records and fit a justified survival model instead. A subgroup trend cannot substitute for component-level time-to-event analysis.
Uncertainty and comparability. Use appropriate confidence intervals for the failure rates and their ratio, and state the statistical model and method. Under a simple event-count model, low counts imply substantial uncertainty; the reference cohort contains only five failures. If counts are clustered by aircraft, operator, production lot, or mission, account for that dependence rather than treating every event as independent.
If individual lifetime observations are available, inspect survival estimates and test the constant-hazard assumption. Adjust or stratify comparisons for important differences in age, temperature, electrical loading, vibration, mission profile, maintenance, and failure ascertainment. Separate descriptive evidence of elevated failures from causal attribution. Do not infer fleet-wide risk or a formal trend from this small illustrative table alone.
Worked solution: supply-chain hypotheses and evidence.
Ordinary quality defect: compare manufacturing records, material and assembly inspection, acceptance tests, and failure analysis across affected and unaffected modules.
Operational overstress: examine temperature, voltage/current, mechanical loading, installation, cooling, and maintenance records using comparable missions.
Counterfeit or substituted parts: trace serial and lot identifiers, procurement and custody records, distributor confirmation, and physical characteristics against trusted reference samples.
Unauthorized firmware or configuration: preserve installed images and settings; compare version, digest, and signature-verification results where implemented; update records; and compare against approved reference artifacts.
Deliberate tampering: identify verified unauthorized modifications and their connection to the observed failures. Additional attribution evidence is required before alleging malicious intent or identifying an actor.
An unexpected digest or physical discrepancy warrants investigation but may also reflect authorized revisions or record errors. Verify those alternatives. Do not install suspect firmware or modified components in an operational aircraft to reproduce the problem; use an appropriately instrumented, isolated bench or emulator for controlled diagnosis.
Containment and accountability. Assign the maintenance authority to assess affected inventory and aircraft, the evidence custodian to preserve failed parts and digital artifacts, the procurement owner to obtain provenance records, and the technical reviewer to approve the investigation findings. Define the scope of any precautionary inventory hold or maintenance action through the responsible authority’s procedures.
Record failure criteria, exposure estimates, analysis revisions, physical and firmware findings, uncertainty, and the basis for any supplier-related conclusion. Specify corrective-action verification and return-to-service evidence separately from the statistical alarm. A safer replacement batch or a lower observed failure rate does not, by itself, establish that the original cause was a cyberattack.
Implementation checklist:

- [ ] Define failure, first-failure counting, exposure, and censoring consistently.
- [ ] Distinguish rising failure rate from decreasing MTTF.
- [ ] State reliability-model assumptions and quantify statistical uncertainty.
- [ ] Compare component age, operating conditions, and observation coverage.
- [ ] Preserve serial/lot provenance, original parts, firmware, and diagnostic records.
- [ ] Evaluate quality defects and operational causes alongside compromise hypotheses.
- [ ] Separate reliability evidence, verified modification, and malicious-intent attribution.
- [ ]  Assign containment, review, corrective-action, and return-to-service responsibilities.

**Expected deliverable:** A reliability comparison using the synthetic data, an explicit account of MTTF assumptions and uncertainty, and a supply-chain investigation plan linking each hypothesis to evidence, responsible roles, and decision criteria. The final report must distinguish an elevated failure rate from a demonstrated supply-chain attack.
**Practical Example:** Quantifying GNSS Spoofing and Detection
This exercise applies the supplied position-bias, innovation, fusion, and error equations to a reproducible GNSS-spoofing simulation. Students calculate an idealized detection time, verify it against noisy observations, and assess mitigation without equating position consistency with proof of authenticity.

### Practical Exercise 10.3: GNSS Spoofing, Innovation Detection, and Mitigation Verification
**Objective:** Determine how a gradually increasing false position affects navigation, when an innovation detector should alarm, and how strongly a fusion gate suppresses the modeled position bias.
Worked situation. Load `GNSS_Spoofing_Carrier_1575_42MHz.json` in the compatible simulator. The scenario contains a synthetic position source (G1), carrier context (G2), spoofing bias (G3), detector/fusion gate (G4), configured-power scope (G5), mitigated scope (G6), and raw scope (G7). The carrier value 1575.42 MHz is context metadata, not a generated RF signal.
Use a 90-frame run with baseline frames 0–14, spoofing exposure frames 15–74, and post-exposure frames 75–89. The exposure ramps over 25 frames with displacement $[35,-20,5]^T$ m. The innovation threshold is 4 m. GPS weights are 0.48 before an alarm and 0.04 when the fusion gate is active. Preserve the source seed and independent-reference configuration across comparisons.
Tasks.
Calculate the maximum displacement and the idealized first alarm frame using the bias and innovation equations.
Export raw position, reference position, innovation, alarm, fusion weight, and error records. Compare the observed first alarm with the idealized result.
Calculate the full-bias fused error in a noise-free reference case and distinguish it from the noisy simulator result.
Compare gated and ungated runs using paired observation windows, RMSE, peak error, detection delay, and baseline false alarms.
Explain which conclusions concern synthetic spoofing detection and which would require independent receiver or flight-controller evidence.
![Figure 3: Synthetic GNSS spoofing and position errors](chapters/Chapter_10/figures/GNSS_Spoofing_ch_10_3.jpg)
AeroResilience synthetic GNSS-spoofing workflow and position-error comparison. The raw observation branch and GPS-like error traces approach approximately 40–41 m, while fusion-gated error remains near 2 m during the sustained bias interval. The horizontal axis is the simulation frame index, not elapsed seconds. Overlapping traces can obscure individual curves.
Worked solution 1: position-bias model. Write the modeled GPS-like observation as

$$
\mathbf{p}^{GPS}_k=\mathbf{p}^{true}_k+\mathbf{b}_k+\mathbf{v}_k,
\qquad
\mathbf{p}^{ref}_k=\mathbf{p}^{true}_k+\mathbf{e}^{ref}_k,
$$

where $\mathbf{v}_k$ and $\mathbf{e}^{ref}_k$ are the modeled observation errors. Within the exposure window,

$$
u_k=\min!\left(1,\frac{k-15+1}{25}\right),\quad
g_k=3u_k^2-2u_k^3,\quad
\mathbf{b}_k=g_k\begin{bmatrix}35&-20&5\end{bmatrix}^{T},\mathrm{m}.
$$

Set $\mathbf{b}_k=\mathbf{0}$ outside frames 15–74. The maximum bias norm is

$$
B=|\mathbf{b}_{\max}|_2=\sqrt{35^2+(-20)^2+5^2}
=\sqrt{1650}\approx40.62,\mathrm{m}.
$$

The ramp reaches its full value at frame 39 under the stated indexing convention and remains at full bias through frame 74. Grouping frames 15–39 as the ramp phase includes that terminal full-bias sample.
Worked solution 2: idealized detection time. The detector uses
$$
r_k=|\mathbf{p}^{GPS}_k-\mathbf{p}^{ref}_k|_2,
\qquad a_k=\mathbb{I}[r_k>4,\mathrm{m}].
$$

With zero observation error, $r_k=g_kB$. At frame 18,
$$
u_{18}=4/25=0.16,\quad g_{18}=0.068608,\quad
r_{18}\approx2.79,\mathrm{m}.
$$

At frame 19,
$$
u_{19}=5/25=0.20,\quad g_{19}=0.104,\quad
r_{19}\approx4.22,\mathrm{m}.
$$

Because the ramp is increasing, the idealized first alarm is frame 19 and the delay from onset is $19-15=4$ frames. This calculation is a noise-free prediction, not a measured alarm time. Noise, reference error, timing misalignment, or a different seed can alter the observed result. Convert the delay to seconds only if a justified physical frame duration exists.
Worked solution 3: mitigation effect. Use the supplied fusion equation:

$$
\widehat{\mathbf{p}}_k=w_k\mathbf{p}^{GPS}_k+(1-w_k)\mathbf{p}^{ref}_k.
$$

The resulting estimation error is

$$
\widehat{\mathbf{p}}_k-\mathbf{p}^{true}_k
=w_k\mathbf{b}_k+w_k\mathbf{v}_k+(1-w_k)\mathbf{e}^{ref}_k.
$$

At full bias with an exact reference and no observation noise, the alarm-gated error is

$$
e_{gated}=0.04B\approx1.62,\mathrm{m}.
$$

If gating is disabled but nominal fusion remains active, the same idealized error is

$$
e_{ungated}=0.48B\approx19.50,\mathrm{m}.
$$

The raw observation error is approximately 40.62 m. These are three distinct comparison quantities. The screenshot’s raw error near 40–41 m and gated error near 2 m are consistent with additional modeled noise, but exact results require exported data. Gating reduces the bias contribution; it does not completely reject GPS because its weight remains nonzero.
Verification and acceptance. Declare the evaluation set $\mathcal{K}$ and use the same $M$ valid paired frames for

$$
\mathrm{RMSE}{raw}=\sqrt{\frac{1}{M}\sum{k\in\mathcal{K}}(e_k^{raw})^2},
\qquad
\mathrm{RMSE}{fused}=\sqrt{\frac{1}{M}\sum{k\in\mathcal{K}}(e_k^{fused})^2}.
$$

Report baseline, ramp, full-bias, and post-bias intervals separately. An optional classroom criterion is detection within 6 frames of onset and a maximum gated error below 3 m during exposure. These illustrative thresholds require mission-specific justification and must be fixed before testing. Do not claim they pass until you verify them through observations.
Run three comparisons: zero bias; spoofing with `gate_enabled=0`; and spoofing with `gate_enabled=1`. Disabling the entire gate block suppresses the branch and is not the intended ungated comparison. Record baseline false-alarm counts and denominator, alarm frame, peak errors, RMSE, and any failed criterion. Repeat over distinct source seeds to evaluate variability; repeating the same seed checks repeatability only.
Interpretation and limits. An innovation alarm establishes disagreement between observations under the stated model. Benign reference drift, sensor faults, multipath-related positioning error, or synchronization problems may also cause disagreement. Distinguish anomaly detection from spoofing attribution and assess reference independence and integrity.
G7 is a raw observation scope, not a detector. G5 shows configured power references, not measured GNSS RF power. Position consistency, carrier metadata, or low fused error does not prove signal authenticity. The exercise does not model satellite pseudoranges, receiver acquisition/tracking, packet delivery, physical flight dynamics, or executed autopilot recovery. Bias removal at frame 75 is a model reset, not verified receiver reacquisition.
Implementation checklist:
  
- [ ] Verify the bias vector, schedule, frame indexing, threshold, and weights.
- [ ] Compare the idealized alarm prediction with exported noisy observations.
- [ ] Separate raw error, nominal fusion, and alarm-gated fusion.
- [ ] Preserve paired inputs and report RMSE, peaks, false alarms, and failures.
- [ ] Validate independent-reference assumptions and retain alternative explanations.
- [ ] Distinguish configured power/carrier context from RF or flight measurements.
- [ ] Preserve JSON, software revision, seeds, CSV records, and reviewer decisions.
- [ ] Expected deliverable. A worked calculation of bias magnitude and idealized alarm time, an annotated figure, a verified raw/ungated/gated comparison, and a report identifying acceptance outcomes, uncertainty, and the limits of the simulated spoofing assessment.
