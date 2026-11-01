# UAS Intelligence, Surveillance, and Reconnaissance Security
ISR security covers everything from collecting data with sensors to processing, sending, storing, analyzing, and sharing that data. At each step, track where and how the data was collected, including the time, platform status, sensor settings, and any later processing. Limit access based on the information’s sensitivity and each user’s responsibilities. When choosing hardware and software, consider factors like data rates, processing deadlines, storage space, communication options, and energy use. Check how security measures might affect data availability and usefulness for analysis. To keep data traceable, record any changes, who accessed the data, and updates to processing models or software.

## Practical Exercises: Evidence Provenance and ISR Mission Resilience
Intelligence, surveillance, and reconnaissance (ISR) workflows require traceable observations and explicit criteria for maintaining useful service under disruption. The following exercises address information provenance and mission-resilience assessment. Synthetic communication packets support an analogy for evidence handling; they do not constitute validated ISR imagery or payload products.

### Practical Exercise 13.1: Freshness, Duplication, and Observation Provenance
**Objective:** Develop a provenance-checking procedure that distinguishes fresh observations from delayed or duplicated information, preserves the original records, and documents processing history.
**Worked situation:** Use `Jamming_Injected_Replay_Attack_2400MHz.json` as an analogy for information handling during communication disruption. Inspect the synthetic packet source, replay settings, and combined evaluation output. Verify which packet identifiers, source references, frame indices, or timestamps the implementation actually exposes. Record missing provenance fields rather than inventing them.
The supporting scenario is provided through the book’s repository:
- [ ] UAV CyberPhysical Security repository. 
- [ ] Practical task.
- [ ] Preserve the original configuration and raw observations, and record simulator and processing-software versions.
Trace synthetic packet generation, replay processing, and the combined output. Determine whether the output preserves separate packet events or only aggregate indicators.
- [ ] Associate each available observation with its source, session or run identifier, sequence identifier, acquisition time or source-frame reference, receipt time where available, and processing history.
- [ ] Identify repeated, delayed, out-of-order, and incomplete observations using declared comparison and timing rules.
- [ ] Produce a provenance ledger and a documented acceptance, flagging, or deferral decision for each observation.

* **Hint:** Arrival time and acquisition time are different. A newly received record may describe an old observation. A repeated sequence identifier may indicate duplication, but interpretation also requires the source, session, counter-wrap behavior, and transformation history. A digest can identify identical bytes without proving source authenticity or acquisition time.
Worked solution: provenance-checking procedure.
* **Preserve the original:** Retain the unmodified input and assign a stable evidence identifier. Record its source and collection context, storage reference, and content digest where used. Perform analysis on linked working records.
Establish identity. Build an observation key from the source, session or run, and sequence identifier where available. Document whether these fields are merely reported or independently verified.
* **Assess freshness:** Compare acquisition and receipt or evaluation time only when they share a documented time basis or have a justified clock mapping. Apply a mission-defined freshness limit and retain timing uncertainty. With frame-based simulation, report age in frames unless a validated time conversion exists.
Identify duplicates. Compare observation keys and, where useful, content digests. Distinguish identical duplicates from records that share an identifier but differ in content. Preserve duplicate links and conflicting records rather than deleting evidence.
* **Record transformations:** For every derived observation, retain parent identifiers, processing time, software or model version, parameters, and relevant changes to content, metadata, coordinates, units, or time references.
Record disposition. Mark an observation as accepted for the stated use, stale, duplicated, conflicting, incomplete, or unresolved according to the defined rules. Retain the reviewer, rationale, and supporting evidence. Freshness alone does not establish authenticity or payload correctness.
 **Payload-specific limits:** The supplied packets represent a communication exercise, not actual ISR imagery or payload products. Image integrity, sensor calibration, geolocation accuracy, acquisition metadata, product quality, and task-specific interpretation require separate payload validation. If the combined evaluation output contains only aggregate metrics, additional packet-event logging is required before you can assess observation-level provenance. Do not claim an implemented authentication defense or validated ISR evidence from this workflow alone.
Implementation checklist:

- [ ] Verify freshness using a documented time basis and threshold.
- [ ] Preserve original records and their collection context.
- [ ] Identify duplicates, conflicts, and out-of-order observations.
- [ ] Document every transformation affecting content or evidence metadata.
- [ ] Separate reported source identity from verified authenticity.
- [ ] Identify missing fields and additional payload-specific validation.

**Expected deliverable:** A provenance ledger and checking procedure linking observations to their source, sequence identifier, acquisition reference, processing history, and review disposition, with explicit limits on conclusions about ISR payload products.

### Practical Exercise 13.2: Mission-Resilience Matrix for ISR UAV Operations
**Objective:** Construct a mission-resilience matrix connecting disturbances, essential ISR functions, acceptance criteria, evidence, recovery actions, and responsible decision-makers.

**Worked situation:** An ISR UAV must preserve an agreed level of useful observation and evidence integrity during communication degradation or component failure. Define the mission profile, essential services, acceptable degraded operation, and decision authority before assigning thresholds. The matrix below is a teaching template, not a certification checklist or evidence that any particular platform satisfies the requirements.

**Practical task:** Complete one assessment record for each matrix row. Assign a requirement identifier, justified threshold, observation interval, owner, evidence reference, observed outcome, and residual risk. Test the supported functions using simulation or recorded observations and mark unsupported functions as not evaluated.
Mission-resilience matrix.
* **Function / disturbance	Assessment, evidence, and response C2 availability / RF disruption:**	Define valid-command delivery and maximum tolerated interruption. Use packet records and controller telemetry; assign the operator to confirm the predefined degraded-operation response.
* **Navigation / degraded positioning:**	Define acceptable position uncertainty and continuity for the mission phase. Retain estimator status and an independent reference where available; require review of navigation recovery before resumption.
* **Payload acquisition / sensor or storage interruption:**	Define usable-product and tolerated-gap criteria. Retain acquisition logs, calibration references, and storage status; assign the payload operator to assess gaps and product validity.
* **Observation freshness / delay or replay:**	Define permitted age and duplicate handling. Use source/session identifiers, acquisition references, and arrival records; flag stale or conflicting information for analyst review.
* **Evidence integrity / alteration or processing error:**	Define required provenance and integrity checks. Preserve originals, derived-record links, and verification results; assign the evidence custodian to investigate discrepancies.
* **Processing / overload or deadline miss:**	Define decision deadlines and valid-data coverage. Measure execution latency, dropped inputs, and memory use; use an approved fallback and record unevaluated observations.
* **Energy / reduced endurance:**	Define reserve and service-duration requirements. Use measured power and energy telemetry with uncertainty; assign the operator to apply the approved mission-energy response.
* **Recovery / persistent or recurring degradation:**	Define sustained recovery criteria for all essential services. Correlate link, controller, navigation, and payload records; require the designated authority to approve mission resumption.
* **Worked solution:** Translate each row into measurable requirements and an executable assessment procedure. Preserve baseline, disturbance, and recovery observations and compare equivalent intervals. Record each result as meeting the criterion, failing it, or not evaluated because evidence or implementation is missing. An untested function must not receive a passing result.
Do not let good performance in one function compensate for failure of an essential service. Declare essential-function gates before **evaluation:** mission acceptance requires every applicable essential criterion to be satisfied and reviewed. Nonessential performance summaries may supplement that decision but must retain their definitions, weights if used, and missing-data treatment. A high aggregate score alone does not establish mission readiness.
For each failure or unresolved result, identify the effect on the ISR mission, an approved degraded-operation or recovery procedure, the responsible owner, and the evidence required for closure. Separate simulation-derived link indicators from measured flight, navigation, payload, and energy results. The communication JSON may support selected exposure tests; it does not implement the complete matrix.
Implementation checklist:
- [ ] Define mission scope, essential functions, and acceptance thresholds.
- [ ] Assign a responsible owner and evidence source to every row.
- [ ] Preserve baseline, disturbance, and recovery observations.
- [ ] Separate tested results, assumptions, and unevaluated functions.
- [ ] Retain provenance and uncertainty for ISR products and decisions.
- [ ] Record residual risk and require the designated authority to approve resumption.

**Expected deliverable:** A completed mission-resilience matrix with requirement identifiers, criteria, owners, evidence references, observed outcomes, recovery decisions, and residual risks, accompanied by a record of functions that require additional instrumentation or validation.
