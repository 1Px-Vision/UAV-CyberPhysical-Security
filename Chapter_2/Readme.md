# Airspace Cybersecurity: Attack Taxonomy and Real-World Incidents

## Incident Reconstruction, Security Failures, and Accountability
Examine real-world incidents through a consistent framework that connects the initiating event, affected subsystem, observed behavior, operational consequences, and available evidence. An incident narrative should distinguish verified observations from inferred attack mechanisms. A communication outage, for example, may result from interference, equipment failure, congestion, or an adversarial action; the observed symptom alone does not establish its cause. Incident analysis should identify which hardware and software components contributed to exposure, which monitoring capabilities were available, and how operators responded. Accountability requires documenting who authorized the mission, maintained the system, reviewed alerts, and approved subsequent corrective actions.

### Practical Example: Reconstructing a Simulated Communication Disturbance
The following exercise uses AeroResilience to examine the relationship between configured interference behavior and observed communication effects. It introduces incident reconstruction, comparison of alternative explanations, and evidence-based reporting.
**Practical Exercise 2.1:** Communication-Disturbance Reconstruction and Accountability
Objective. Reconstruct a simulated communication-disturbance timeline, compare temporal interference patterns, and distinguish scenario configuration from observed effects and inferred causes.
Worked situation. A simulated UAV communication link experiences intermittent degradation. Load `Friendly_Spot_Intermittent_2400MHz.json` to investigate a scenario combining spot interference with intermittent activation. Examine how the configured timing relates to changes in the communication outputs.
The scenario files are provided through the book’s repository:

https://github.com/1Px-Vision/UAV-CyberPhysical-Security

**Practical procedure**
Preserve the original JSON file and record the software revision, simulation settings, and random seed where available.
Inspect the interference and timer blocks. Record the configured activation intervals, durations, and repetition settings where available.
Run the simulation over a defined observation interval. Record available output power, SNR, SINR, expected PDR, and corresponding simulation timestamps or frame indices.
Reconstruct the disturbance timeline by comparing configured activation periods with observed changes in the outputs.
Repeat the analysis using `Friendly_Spot_Continuous_2400MHz.json` and `Friendly_Spot_Random_2400MHz.json`. Hold other parameters fixed where possible and document any differences between the configurations.
Prepare an incident report separating configuration evidence, observed communication effects, alternative explanations, and conclusions.
Illustrated scenario. Figure 1 provides the scenario view used to examine the relationship between block configuration and communication outputs.
![Figure 1: Communication-disturbance scenario](https://github.com/1Px-Vision/UAV-CyberPhysical-Security/blob/main/Chapter_2/Communication-disturbance%20scenario.jpg)
AeroResilience scenario view supporting reconstruction of a simulated communication disturbance. Compare the configured interference and timing parameters with available communication outputs over a defined observation interval.
Hint. Treat the JSON file as evidence of the configured simulation behavior. Treat recorded scope and metrics outputs as evidence of the simulated communication response. A reduction in PDR or SINR alone does not establish an interference mechanism or malicious intent in a real incident.
Worked solution. Construct a timeline listing each configured interference interval alongside the corresponding observed communication effects. Identify whether output changes coincide with activation, deactivation, and recovery periods. Document any delay, persistence, or mismatch rather than assuming an immediate response.
Compare the intermittent scenario with the continuous and random companion scenarios. Determine whether the recorded outputs follow their respective configured temporal patterns. Where random behavior is implemented, preserve the seed where available and repeat the observation to characterize variability.
Figure 2 supports inspection of the recorded simulation outputs. Interpret differences only after checking that observation intervals, reference points, and remaining scenario parameters are comparable.
![Figure 2: Simulated communication outputs](chapters/Chapter_2/figures/Practical_p2_ch_2.jpg)
AeroResilience output views used to inspect simulated communication effects during disturbance reconstruction. Interpretation requires alignment with the configured timing and the observation interval; RF symptoms alone do not establish malicious intent.
Incident report and accountability. Organize the report into the following elements:
Configuration evidence: JSON filename, revision, interference parameters, timer settings, and relevant model assumptions.
Observed effects: Timestamped or frame-indexed changes in the available communication outputs.
Alternative explanations: Discuss whether similar symptoms could arise from congestion, fading, equipment malfunction, or other interference, and identify the additional evidence required to distinguish these causes.
Supported conclusions: State what the simulation demonstrates, the evidence supporting each conclusion, and the remaining uncertainty.
Responsibilities: Identify who configured the scenario, collected the outputs, reviewed the interpretation, and approved proposed corrective actions.
The completed report separates configured interference behavior from observed communication effects. It does not infer malicious intent from RF symptoms alone. Reproducing a disturbance in simulation demonstrates a possible explanation under stated assumptions; it does not establish the cause of a real-world incident.
Implementation checklist:

- [ ] Preserve the original JSON configurations and software revision.
- [ ] Record observation intervals and align timestamps or simulation frame indices.
- [ ] Compare configured timing with observed degradation and recovery.
- [ ] Document differences among intermittent, continuous, and random scenarios.
- [ ] Compare alternative explanations and identify missing evidence.
- [ ] Identify the evidence supporting each conclusion.
- [ ] Record reviewer responsibilities and corrective-action ownership.
  
**Expected deliverable:**  An incident report containing an annotated disturbance timeline, a comparison of the three scenarios, supporting observations, alternative explanations, and clearly assigned review responsibilities.
Practical Example: Wi-Fi C2 Capture and Simulated Command Injection
Packet capture can reveal communication patterns and support incident reconstruction, but observing a packet does not equal gaining aircraft control. This exercise uses an owned, isolated Wi-Fi test link and a drone emulator to distinguish passive observation, packet delivery, command validation, and simulated state changes.

***Practical Exercise 2.2: Wi-Fi C2 Sniffing and Simulated Control Attempts*** 
**Objective:** Capture and interpret a controlled command-and-control (C2) exchange, simulate an unauthorized command attempt in a drone emulator, and reconstruct the evidence required to determine whether the command was received, rejected, or executed.
Worked situation. A laboratory ground-control application exchanges synthetic commands and telemetry with a software drone emulator over an owned Wi-Fi network. A passive capture process records the test traffic. A separate test client supplies synthetic unauthorized and replayed commands to the emulator’s documented test interface. No operational aircraft is controlled, and the exercise does not require discovering third-party devices, obtaining credentials, disrupting Wi-Fi associations, or bypassing a real drone’s protections.
Here, “capture” means recording network observations; simulated control takeover is demonstrated only as an explicitly labeled emulator state change. Do not describe a successful packet capture as aircraft takeover.

**Laboratory elements**
An owned Wi-Fi access point and two test hosts, or equivalent isolated virtual hosts, with a documented topology and test address list.
A ground-control emulator that produces synthetic C2 messages and a drone emulator that exposes a documented laboratory command interface.
A packet analyzer such as Wireshark and capture capability on an authorized endpoint or test-network observation point.
A test client producing only the exercise messages, together with application logs and a shared or documented time reference.
Endpoint capture is sufficient for the application-level exercise. A capture taken after endpoint decryption is not evidence that an outside observer could recover protected Wi-Fi traffic. If you collect radio-layer observations separately, document the adapter, capture mode, available metadata, and visibility limits. This exercise includes no credential-recovery step.
Synthetic command record. Define a teaching message containing a source identifier, session identifier, command identifier, sequence number, generation time, command value, and validation result. Use harmless emulator actions such as requesting status or changing a virtual mission-state label. These fields are an exercise protocol, not a claim about any commercial drone’s C2 format.
Define two receiver profiles:
Deliberately permissive emulator: a documented insecure test mode that accepts a syntactically valid laboratory message without sufficient source or freshness validation.
Validated emulator: a separately implemented profile that checks an authenticated session or message, authorization, freshness, sequence handling, and allowed state transitions according to its stated design.
Do not invent a cryptographic protocol for production use. If you evaluate authentication, identify the actual library or protocol configuration and document what protection it provides. Do not treat link encryption alone as evidence of application-level command authorization.
Practical procedure.
Record the owned test devices, software versions, topology, capture point, emulator configuration, and test interval. Preserve the original configurations.
Start the passive capture at the approved observation point and run a baseline exchange of synthetic commands and telemetry.
Restrict the analyzer display to the known test endpoints and configured laboratory C2 transport port. Document the display filter and do not infer that filtered-out traffic was absent.
Identify direction, timestamps, packet lengths, message cadence, and exposed command identifiers where visible. Retain application logs when payload contents are protected or unavailable.
In the permissive emulator, submit one synthetic command from the test client and observe whether the emulator records acceptance and a virtual state change.
Repeat with the validated profile using an unauthorized test message, an exact duplicate of a previously accepted synthetic message, an expired message, and a valid authorized message. Document the expected outcome for each case before execution.
Correlate packet observations, receiver decisions, acknowledgments, and emulator state logs. Compare profiles under equivalent test conditions.
Illustrative sniffer observation. A baseline capture might show periodic control messages from the ground-control host to the emulator and telemetry in the reverse direction. A later test message may reach the same emulator interface from the laboratory test client. That observation establishes only that traffic was visible at the capture point. Establishing command acceptance requires receiver evidence; establishing execution requires a corresponding emulator state event.

For example, retain the following event chain with actual run timestamps:
- [ ] The test client generates synthetic command `TEST-001`.
- [ ] The capture records the associated test flow at the documented observation point.
- [ ] The receiver log records the command identifier and an accept-or-reject decision with a reason.
- [ ] The emulator records a virtual state transition only when the command executes.

An acknowledgment alone does not prove the intended state change. If packet contents cannot be mapped to a command identifier, explain the limits of correlation rather than claiming a definitive association.
Hint. Separate four questions: was traffic observed, did it reach the receiver, did validation accept it, and did the emulator execute it? Missing telemetry can reflect capture loss, delay, application failure, or logging limitations. Do not automatically classify it as hostile activity or a successful takeover.
Worked solution. Produce a case-by-case comparison of the permissive and validated emulator profiles. The permissive profile illustrates the consequence of an intentionally absent validation function; it does not establish a vulnerability in a commercial aircraft. Assess the validated profile against its actual implementation and logs, rather than assuming it rejects every test message.
For each test, record the message identity, session, sequence information, generation and receipt times where available, capture reference, receiver decision, rejection reason, acknowledgment, and virtual state outcome. Mark absent or ambiguous evidence as unresolved. Preserve the raw capture and original logs, and document any extraction, redaction, timestamp conversion, or packet reassembly applied during analysis.
When testing replay handling, confirm the emulator accepted the original authorized message before replaying its duplicate. Evaluate whether the duplicate is rejected, and ensure the rejection is due to receiver validation rather than packet loss. A stale message test must have a documented freshness rule and time basis. Record counter resets and session changes because duplicate interpretation depends on them.
Incident reconstruction and accountability. Construct a timeline separating the test-client action, network observation, receiver validation, and emulator state change. Identify who authorized the test, operated the capture, configured each profile, reviewed the evidence, and approved corrective actions. Report the demonstrated security failure at the emulator’s application layer and avoid generalizing it to unrelated Wi-Fi equipment or real aircraft.
A suitable conclusion is: “Under the documented permissive configuration, the emulator accepted the synthetic test command and recorded the specified virtual state change.” For the validated profile, report the actual acceptance/rejection outcomes and remaining gaps. Neither conclusion demonstrates control of a physical drone.
Evaluation metrics. Report valid-message acceptance, unauthorized-message acceptance, duplicate acceptance, stale-message acceptance, and legitimate-message rejection using explicit denominators of messages actually presented to the validator. Count network losses separately. Measure validation latency, end-to-end emulator response time, and capture loss or incomplete evidence where observable. Do not infer these execution costs from packet format alone.

Implementation checklist:

- [ ] Use owned test equipment and an isolated emulator-based command interface.
- [ ] Record capture location, visibility limits, filters, and timing references.
- [ ] Distinguish packet observation, receiver acceptance, and simulated execution.
- [ ] Preserve original captures, message identifiers, and receiver/state logs.
- [ ] Evaluate authentication, authorization, freshness, and state validation separately.
- [ ] Record losses, ambiguous correlations, and unsupported conclusions.
- [ ] Assign review responsibilities and retain corrective-action evidence.
- [ ] Expected deliverable. An annotated packet-capture report and incident timeline comparing permissive and validated drone-emulator behavior, supported by receiver decisions and virtual-state records. Include demonstrated limits, measured validation costs, and recommendations tied to the observed failure mechanisms.
