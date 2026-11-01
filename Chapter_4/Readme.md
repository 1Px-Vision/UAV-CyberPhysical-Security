# UAS Data Links: Functions, Attributes, Latency, and Security Constraints

Data-link security introduces processing, storage, and communication overhead that must be evaluated alongside confidentiality, integrity, authenticity, and availability objectives. Authentication and encryption may increase message size and processing time, while key management and replay protection require additional state and synchronization. Assess these costs against the timing requirements of command, telemetry, payload, and traffic coordination. Evaluation should distinguish algorithmic complexity from measured implementation performance. Relevant measurements include message-processing latency, throughput, memory consumption, processor utilization, and energy expenditure. Results should identify the hardware platform, software implementation, packet size, traffic rate, and test conditions.

## Practical Example: Assessing Asymmetric Anchor-to-Target Links
Examine communication availability at the individual-link level when anchors experience different exposure conditions. An aggregate indicator may obscure a degraded link, particularly when its definition combines results across branches or reports only the selected output. This exercise uses AeroResilience to preserve link-specific evidence and develop explicit criteria for choosing a usable communication path.

### Practical Exercise 4.1: Asymmetric Links and Communication-Path Selection
Objective. Compare two anchor-to-target links under different modeled interference exposures, determine the limits of aggregate reporting, and distinguish modeled communication performance from implementation costs requiring additional measurement.
Worked situation. Load `D2D_Asymmetric_Links_2400MHz.json`. The scenario description identifies two link-evaluation branches exposed to different interference-reference levels and a selector to inspect each branch individually. Confirm the branch identities, reference settings, selector behavior, and output connections in the loaded scenario before interpreting results.
The supporting scenario is provided through the book’s repository:
- UAV CyberPhysical Security repository
-Practical procedure.

Preserve the JSON configuration and record the software revision, model assumptions, and random seed where applicable.
Identify the Anchor 1–Target and Anchor 2–Target branches. Record their configured interference-reference levels and, where available, relevant signal, noise, bandwidth, antenna, and channel settings.
Trace each branch to the selector and output scopes. Verify which selector setting corresponds to each anchor and whether switching the selector changes routing, execution, or both. Define equivalent observation windows and link-acceptance criteria before execution. Specify the statistic used to evaluate each window and how you handle missing outputs. Select each branch individually and retain separate performance records, including expected PDR, SNR, SINR, and $J/S$ where reported. Record the selected branch and simulation time or frame index with every observation. Examine any aggregate availability indicator. Determine its definition and whether it combines both links, represents one selected link, or follows another rule. If no aggregate is provided, construct an explicitly labeled analytical comparison from the retained records. Propose and justify a usable-path selection rule. State which additional observations would be required to evaluate implementation latency, encryption overhead, and energy consumption.
Illustrated configuration. The configuration image below provides the configuration view used to identify the two branches and verify output selection.
![AeroResilience asymmetric-link assessment](chapters/Chapter_4/figures/Practical_p1_ch_4.jpg)
AeroResilience configuration view for assessing asymmetric anchor-to-target links. The two link-evaluation branches and selector support separate inspection of each communication path under its configured interference exposure.

**Hint:** A selector that displays one branch at a time does not necessarily provide simultaneous evidence for both links. Compare the same recorded simulation interval, or use reproducible repeated runs with equivalent settings where supported. If you cannot establish equivalence, document the limitation instead of treating the observations as synchronized.

**Worked solution:** Create independent records for Anchor 1–Target and Anchor 2–Target. Each record must retain the branch identifier, observation interval, configuration reference, reported metrics, acceptance decision, and supporting evidence. Evaluate both links using the same justified criteria, unless a documented mission requirement calls for different treatment.
An aggregate indicator can conceal degradation if strong performance on one branch numerically compensates for poor performance on the other. For illustration only, suppose two equivalent windows yield expected PDR values of $98%$ and $42%$. Their unweighted mean is $70%$, which would satisfy a hypothetical aggregate threshold of $65%$, even though the second link would fail that same threshold individually. These numbers are an instructional example, not outputs claimed for the supplied JSON scenario. An average of link-level expected PDR values is also not automatically a system-availability probability.

Whether one usable path is sufficient depends on the required function. Communication that can be routed through either anchor may tolerate loss of one path, whereas a function requiring observations from both anchors may fail when either link degrades. State this dependency before accepting an aggregate availability result.
The output image below provides the output view used to retain and compare link-specific observations.
![AeroResilience asymmetric-link assessment](chapters/Chapter_4/figures/Practical_p4_ch_4.jpg)
AeroResilience output view supporting link-specific performance assessment. Each observation is associated with the selected branch and its observation interval; comparative conclusions require equivalent windows and documented selection settings.
Proposed usable-path criteria. The following rule is an assessment proposal rather than a claim that automatic switching is implemented in the JSON:

- Declare a path eligible only when its output is valid, sufficiently recent, and satisfies the predefined link-performance criterion over the required observation window.
- When both paths are eligible, choose using a documented preference, such as the larger margin above the acceptance threshold, while preserving the records for both branches.
- If automatic selection is later implemented, introduce a justified persistence interval or switching margin, and evaluate its effect on unstable switching.

When neither path is eligible, report that no path meets the criterion and invoke the mission-defined degraded-operation response.
Implementation cost and additional instrumentation. The JSON described for this exercise does not explicitly model processing latency, encryption overhead, or energy consumption. You cannot infer them from expected PDR, SNR, or SINR alone. A hardware implementation requires additional instrumentation:

**Processing latency:** instrument defined software or hardware events with timestamps, tracing, or logic-analyzer observations; document clock alignment and the measured interval.

**Encryption overhead:** compare matched workloads with and without the specified security processing, recording processing time, packet expansion, memory use, and relevant communication settings.

**Energy consumption:** measure voltage and current over a defined operating interval using a suitable power monitor or data-acquisition instrument.

Record these quantities separately from simulation-derived link quality and document the test configuration connecting the two evidence sets.
Implementation checklist:

- [ ] Verify branch identities, selector settings, and output routing.
- [ ] Compare equivalent observation windows and record timing alignment.
- [ ] Preserve separate performance records for both anchor-to-target links.
- [ ] Define the aggregate indicator before interpreting it.
- [ ] State whether the mission function requires either path or both paths.
- [ ] Justify acceptance thresholds and usable-path selection criteria.
- [ ] Distinguish modeled link quality from measured implementation cost.
- [ ] Document missing outputs, model limitations, and additional instrumentation requirements.

**Expected deliverable:** A link-specific assessment containing separate records for both branches, an explanation of any information concealed by aggregation, proposed usable-path selection criteria, and a measurement plan for latency, encryption overhead, and energy consumption.

## Practical Example: Bayesian Detection of DTN Forwarding Attacks
Delay-tolerant networking (DTN) uses store-and-forward operation when end-to-end connectivity is intermittent. This exercise develops a Bayesian assessment of suspicious forwarding behavior while accounting for legitimate contact gaps, congestion, storage limitations, and bundle expiry. It is an additional teaching model, not a capability assumed to exist in the RF JSON scenarios.

### Practical Exercise 4.2: Bayesian Detection of Suspicious DTN Forwarding
**Objective:** Implement and evaluate a Bayesian detector for selective bundle dropping, distinguish forwarding evidence from missing observations, and document the uncertainty and operational cost of a detection decision.
Worked situation. A UAV source forwards observation bundles through a relay to a destination during intermittent contacts. Some bundles are not observed progressing after the relay accepts them. Compare a benign hypothesis $H_0$ with a suspicious selective-dropping hypothesis $H_1$. In a controlled simulation, introduce labeled dropping behavior at the relay for evaluation, while retaining benign cases involving missed contacts, exhausted storage, congestion, and expiry.
A delayed or absent delivery report alone does not establish malicious dropping. Use a separately implemented DTN simulator, an instrumented bundle-forwarding model, or suitable recorded logs. RF disturbance scenarios may supply contact-availability inputs only through a documented interface; they do not implement bundle routing or this detector by themselves.

**Practical procedure:**
Define source, relay, and destination nodes; routing policy; contact opportunities; link capacity; bundle lifetime; offered traffic; and queue capacity. Preserve event records for bundle identity, acceptance, eligible forwarding opportunities, forwarding attempts, expiry, deletion, and destination delivery where observed. Define the observation unit and eligibility rule before testing. Exclude unresolved opportunities from scored forwarding failures and retain the reason for exclusion. Estimate benign and attack likelihoods from separate labeled development runs, conditioned on relevant contact and queue conditions. Choose and justify a prior attack probability. Compute posterior attack probabilities and apply a predefined alarm threshold on held-out runs. Evaluate false alarms, missed attacks, detection delay, delivery effects, and computational overhead. Retain alternative benign explanations and review responsibilities.

**Hint:** Count an eligible forwarding opportunity only when the relay had accepted a valid bundle, a usable contact and adequate transfer opportunity existed under the declared model, forwarding was required by policy, and sufficient observation coverage exists. If downstream visibility is incomplete, treat the outcome as unknown or explicitly model the probability of observing forwarding. Do not equate unobserved forwarding with verified dropping.

**Worked solution evidence definition:** For a simplified detector, let $F$ be the number of unexplained forwarding failures and $S$ the number of observed forwarding successes among eligible, sufficiently observed opportunities. Retain the underlying bundle identifiers and events. Define when an opportunity is considered complete and avoid counting repeated observations of one bundle as independent evidence without a justified model. Reference labels for injected attacks must come from the experiment controller and remain separate from detector inputs. Record contact gaps, storage rejection, expiry, and unavailable telemetry independently. If you cannot distinguish these, the exercise may detect suspicious behavior but cannot reliably attribute its cause.

**Worked solution Bayesian model:** Let $\pi=P(H_1)$ be the prior probability of the attack hypothesis. Under a simplified conditional-independence model, let $p_0$ and $p_1$ denote the failure probability per eligible opportunity under $H_0$ and $H_1$, respectively, with $0<p_0<p_1<1$. Then

$$
P(H_1\mid F,S)=
\frac{\pi p_1^F(1-p_1)^S}
{\pi p_1^F(1-p_1)^S+(1-\pi)p_0^F(1-p_0)^S}.
$$

The model assumes evidence units are conditionally independent with fixed probabilities over the evaluated window. Shared outages and queue episodes can violate this assumption. Group correlated events into justified units, condition on operating context, or use a richer model; otherwise report that the posterior may be overconfident.
For numerical stability, compute the posterior log-odds:

$$
\ell=\log!\left(\frac{\pi}{1-\pi}\right)
+F\log!\left(\frac{p_1}{p_0}\right)
+S\log!\left(\frac{1-p_1}{1-p_0}\right),
$$

$$
P(H_1\mid F,S)=\frac{1}{1+\exp(-\ell)}.
$$

If probabilities vary by observation context, use the corresponding context-dependent likelihood ratios rather than fixed $p_0$ and $p_1$.
Illustrative calculation. For teaching purposes only, assume $\pi=0.10$, $p_0=0.10$, $p_1=0.60$, and a completed observation window with $F=3$ failures and $S=2$ successes. These values are illustrative, not measured operating characteristics. The posterior is

$$
P(H_1\mid 3,2)=
\frac{0.10(0.60)^3(0.40)^2}
{0.10(0.60)^3(0.40)^2+0.90(0.10)^3(0.90)^2}
\approx0.826.
$$

With an illustrative alarm threshold $\tau=0.90$, this completed window would not trigger an alarm. Define whether the actual detector evaluates completed windows, rolling windows, or every eligible event: intermediate posteriors depend on event order and can cross a threshold even when the final-window posterior does not. Avoid repeatedly adding the same evidence when windows overlap.

**Thresholds and response:** Choose $\tau$ using development data and the mission consequences of false alarms and missed attacks. Report sensitivity to the prior, likelihood estimates, and threshold. A posterior is conditional on the stated model and does not prove intent. Define escalation to operator review, additional observation, or a separately validated routing response. Do not automatically isolate a relay solely from an unvalidated posterior, since isolation may itself reduce connectivity.

**Evaluation and computational costs:** Partition development and evaluation observations by independent runs or contact realizations. Use matched contact schedules and traffic for baseline comparisons and repeat across seeds where supported. Report false-positive and false-negative counts, class support, and the corresponding rates for the declared decision unit. Include benign stress cases and model mismatch, not only ideal benign traffic.

Measure detection delay from labeled attack onset to the first qualifying alarm; report undetected attacks separately rather than assigning zero delay. Since DTN may offer no usable contact for long periods, report both elapsed time and the number of eligible opportunities to alarm where meaningful. Account for delayed or missing telemetry and do not interpret contact-free time as forwarding evidence.
Measure detector update latency, peak memory, retained per-relay state, and logging or reporting overhead on the execution platform. Evaluate the effect of any implemented response on valid delivery, expiry, and delay. A detector-only exercise must not claim improved routing or recovery without testing the associated controller.

Protocol and evidence limits. If Bundle Protocol version 7 is used, consult RFC 9171 for bundle handling and optional status reporting. Status reports may be delayed or unavailable and must not be assumed to provide a complete forwarding trace. Consult RFC 9172 for BPSec integrity and confidentiality services; these services do not themselves establish that a relay forwarded a bundle. Verify the authenticity and coverage of the evidence sources separately from the behavioral detector.
Implementation checklist:
- [ ] Define eligible opportunities and distinguish unknown outcomes from failures.
- [ ] Preserve bundle identities, contact records, expiry, and queue evidence.
- [ ] Justify priors, likelihoods, independence assumptions, and alarm thresholds.
- [ ] Separate attack labels from detector inputs and development from testing.
- [ ] Evaluate benign outages, congestion, storage limitations, and missing reports.
- [ ] Report false alarms, missed attacks, delays, calibration limits, and execution costs.
- [ ] Assign review authority and record any routing-response effects separately.

**Expected deliverable:** A Bayesian detector specification, a reproducible DTN experiment, a worked posterior calculation, and an evaluation report supported by event records. State what the evidence supports, which benign explanations remain possible, and which responses require additional implementation or human review.
Protocol references:
* RFC 9171, Bundle Protocol Version 7: https://www.rfc-editor.org/rfc/rfc9171.html.
* RFC 9172, Bundle Protocol Security: https://www.rfc-editor.org/rfc/rfc9172.html.
