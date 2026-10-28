# Open Challenges and Future Research Directions

## Open Challenges in Resource-Efficient, Auditable, and Standards-Aligned UAS Security

Future research on UAS security should link detection and mitigation results with how practical and accountable these solutions are in real-world use. Key challenges include reducing computational and energy demands, maintaining performance as conditions change, coordinating defenses across locations, and preserving critical evidence when communication or storage is limited. To compare research fairly, use clear workloads, typical hardware, repeatable software setups, and consistent ways to measure results. Standards-based evaluations also need stronger links between what organizations want to achieve, the technical steps taken, mission goals, and proof that these goals are met. For autonomous defenses, it is especially important to define action limits, when humans should step in, and how to trace decisions.
Practical Example: Research Protocol for Overlapping and Variable Disturbances
A reproducible resilience study connects a measurable research question to an explicit baseline, a separately implemented defense, controlled exposure conditions, and retained evidence. This exercise uses three AeroResilience scenarios to develop a protocol for overlapping, pulsed, and random disturbances without assuming that the scenario names demonstrate an implemented defense or successful resilience.
Practical Exercise 16.1: Research Design for Overlapping, Pulsed, and Random Disturbances
Objective. Formulate and test a measurable resilience hypothesis using baseline observations, a specified defense, repeatable disturbance conditions, computational budgets, and accountable research records.
Worked situation. Compare `D2D_Multiple_Interferers_2400MHz.json`, `D2D_Transient_Resilience_2400MHz.json`, and `D2D_Unpredictable_Exposure_2400MHz.json`. According to the supplied scenario description, the multiple-interferer workflow schedules noise, spot, and tone branches with partially overlapping exposure windows. Inspect the other workflows to confirm their implemented pulsed and random behavior, timing, routing, and parameter meanings before assigning condition labels.
Use the changing disturbance combinations to formulate a research problem. Verify which branches actually contribute to the evaluated output: graphical overlap and temporal overlap alone do not establish additive interference at the receiver. These workflows provide exposure inputs, not automatic evidence of a defensive mechanism.
Supporting scenarios are provided through the book’s repository:

https://github.com/1Px-Vision/UAV-CyberPhysical-Security

Practical procedure.
Preserve the three original files and record simulator versions, node types, timer parameters, randomization settings, and output selection.
Reconstruct scheduled exposure windows and actual modeled activity. Identify single-source intervals, overlapping combinations, pulse gaps, random events, and, where available, interference-free intervals.
Define the baseline configuration, proposed defense, primary outcome, improvement criterion, and computational budget before evaluation.
Implement or identify the additional defense explicitly and verify that it changes the intended receiver or controller function.
Run matched baseline and defended trials, repeat across independent realizations where supported, and retain the full run inventory.
Analyze variability, resource use, failure conditions, and accountability records; distinguish supported findings from unsupported assumptions.
Illustrated exposure structure. Figure 1 shows the scheduled branches and changing disturbance combinations.
![Figure 1: Proposed exposure structure](chapters/Chapter_17/figures/Practical_p1_ch_16.jpg)
Proposed AeroResilience exposure view for overlapping noise, spot, and tone branches and complementary pulsed or random conditions. Actual receiver exposure requires verification of waveform activity, routing, and combination semantics.
Hint. If the receiver or selector follows one branch rather than summing active branches, the experiment concerns selection among exposures rather than simultaneous additive interference. Record that behavior and revise the hypothesis or implement the required combination function. Do not describe an unsupported summed-waveform model as an observed property of the JSON.
Worked solution: research question and hypothesis. A suitable question is: “Does the specified defense improve the declared communication-performance outcome across verified overlapping, pulsed, and random exposures while satisfying the processing deadline and memory budget?” Specify the defense and its mechanism; a scenario labeled resilience does not define it.
Choose one primary outcome, such as valid packet-delivery proportion over a defined test interval, or explicitly identified expected model PDR if packet counting is unavailable. Define a practically meaningful improvement $\delta$ before testing. For paired run $r$, write
$$
\Delta_r=Q_{\mathrm{defense},r}-Q_{\mathrm{baseline},r},
$$
where $Q$ has a documented definition and unit. For this larger-is-better outcome, evaluate whether the improvement is consistent with the declared target and resource limits. If an outage-duration metric is chosen instead, define its direction and decision rule explicitly. Do not replace a failed primary hypothesis with a favorable secondary metric after observing results.
Worked solution: baseline and exposure control. Preserve an interference-free reference and an exposed baseline with the proposed defense disabled or absent. Compare the defended configuration with the same exposed baseline using equivalent legitimate traffic, receiver settings, channel assumptions, observation duration, and initial state. Document unavoidable differences and any additional sensing or hardware resources the defense introduces.
Record the active disturbance set for each observation interval. Verify whether powers are source settings, modeled reference values, or received in-band quantities. Document normalization and combination rules so overlapping exposures are not compared with a single source under an unstated power change. Retain pulse gaps and random inactive intervals; do not select only favorable active windows.
Worked solution: proposed defense and verification. Select an appropriate research intervention, such as an independently implemented interference-aware receiver filter or communication-selection policy. Specify its inputs, algorithm version, thresholds, activation rule, and required state. Freeze parameters before evaluation or define any adaptation rule in advance. Use separate development observations for tuning.
Verify function execution using diagnostic cases and instrumented state or output records. Account for desired-signal distortion, decision errors, switching transients, or other costs relevant to the chosen mechanism. If you have not implemented the defense, submit a design and validation protocol rather than claiming measured improvement.
Repeatable evaluation conditions.
Define the run length, warm-up treatment, scenario order, initial state, observation windows, and all application overrides.
Use the same disturbance and channel realization within each baseline/defense pair where supported. Use independently selected realizations across pairs to assess variability.
Record seeds and their scope. If reproducible randomization is unavailable, state the limitation and use a documented alternative comparison design.
Choose the number of independent runs using a stated precision or variability rationale. Report it with every result and avoid treating correlated frames from one run as independent experimental repeats.
Report per-scenario and per-combination outcomes, dispersion, and uncertainty intervals, including the calculation method. Preserve run-level differences and explain incomplete or failed runs.
Computational budgets. Declare a maximum processing or decision latency and memory budget consistent with the intended platform. Measure actual execution costs with documented input dimensions, window sizes, software dependencies, thread settings, and accelerator configuration. Report typical and tail latency, memory peaks, and deadline misses where applicable. Separate algorithm time from buffering, data transfer, rendering, and acquisition. JSON complexity alone does not establish computational performance. Measure energy separately if it is part of the research question.
Illustrated evaluation records. Figure 2 shows observations supporting paired comparisons and condition-specific analysis.
![Figure 2: Proposed evaluation records](chapters/Chapter_17/figures/Practical_p2_ch_16.jpg)
Proposed observation view for baseline and defended evaluations. Performance claims require verified defense execution, matched conditions, repeated-run analysis, measured computational costs, and retained unsuccessful cases.
Accountability and reporting. Assign responsibility for scenario preparation, defense implementation, run execution, data preservation, and independent review. Retain the protocol revision, exact configurations or digests, software and model versions, seeds, run logs, analysis scripts, inclusion rules, and evidence references. Document protocol changes and identify whether they occurred before or after results were examined.
Report improvements, negligible effects, degradations, deadline violations, and failed or unsupported conditions. Explain exclusions and retain their records. State whether findings concern simulation, a software execution platform, or separately validated RF hardware. Avoid generalizing beyond the tested waveform, channel, receiver, and disturbance assumptions.
Implementation checklist.
Identify unsupported assumptions and verify actual exposure combinations.
Define measurable hypotheses, primary outcomes, and acceptance criteria before testing.
Preserve interference-free and exposed baselines.
Verify the separately implemented defense and record its resource requirements.
Evaluate variability across independent repeated runs using matched pairs where supported.
Measure latency, memory, and deadline compliance on the execution platform.
Retain accountability records, analysis provenance, and protocol revisions.
Report improvements and unsuccessful cases without selective omission.
Expected deliverable. A research protocol specifying baseline performance, a proposed and explicitly identified defense, repeatable evaluation conditions, computational budgets, and accountability records, accompanied by condition-specific results where execution is completed, and an explicit record of unsupported assumptions and unsuccessful cases.
Practical Exercises: Toward Trustworthy, Resilient, and Ethical UAV Ecosystems
A HAPS-assisted UAV ecosystem connects aircraft, relay services, operators, traffic-management services, suppliers, and data recipients. The following exercises apply the chapter’s assurance and governance equations without assuming that a reliable communication path automatically establishes trustworthy autonomy or ethical data use. HAPS propagation and platform constraints require separate models; the equations here address decision assurance and assessment.
Practical Exercise 16.2: Runtime Assurance and Resilient Decisions in a HAPS-Assisted Mission
Objective. Evaluate an AI-generated command using a state-dependent safety gate, calculate an electromagnetic risk index, and decide how relay and navigation degradation should affect permitted autonomy.
Worked situation. A disaster-response UAV communicates through a HAPS relay. A learning-based planner proposes a motion command while telemetry delay and position inconsistency increase. The safety monitor, relay-quality observations, and operator escalation policy must be evaluated separately from the planner’s output.
Use Eq. `eq:runtime_assurance_ai_uav` for the command-selection rule. For this classroom model, let $\mathbf{u}=[v_x,v_y,v_z]^T$ in m/s and define a state-dependent admissible set requiring horizontal speed no greater than 3 m/s, vertical speed magnitude no greater than 1 m/s, and a predicted next state inside the approved virtual boundary. Membership is evaluated from current state and uncertainty, not from command magnitude alone.
Tasks.
Apply Eq. `eq:runtime_assurance_ai_uav` to the proposed command and record the safety-monitor decision.
Normalize the observations entering Eq. `eq:em_risk_score` and calculate the risk index.
Use Eq. `eq:em_degraded_mode_decision` as a candidate mode-selection policy, then verify the preconditions for the selected response.
Specify HAPS link, navigation, controller, and operator evidence required before accepting a mode transition or recovery.
Worked solution: runtime assurance. Suppose $\mathbf{u}{AI}=[4,0,0]^T$ m/s. The horizontal-speed requirement fails, so Eq. `eq:runtime_assurance_ai_uav` selects the fallback command. A zero-velocity request $\mathbf{u}{safe}=[0,0,0]^T$ is a possible emulator fallback only if the monitor’s model confirms that it is admissible for the current state. A numerical zero command does not itself prove hover, stability, or collision avoidance.
Retain the proposed command, violated constraint, relevant state uncertainty, fallback command, execution acknowledgment, and observed state. The equation describes an assurance architecture; it does not certify the safe set or verify a controller without supporting evidence. Stale relay telemetry may make the state estimate too uncertain to authorize either proposed action, requiring a separately defined local fallback or escalation.
Worked solution: electromagnetic risk. Equation `eq:em_risk_score` combines quantities with different units. For this exercise, replace each raw input with a documented normalized severity in $[0,1]$, preserving the raw measurement separately. Let the normalized packet loss, interference, GNSS inconsistency, and link-delay severity be $(0.30,0.80,0.60,0.50)$ and choose nonnegative weights $(0.30,0.25,0.25,0.20)$. Then

$$
J=0.30(0.30)+0.25(0.80)+0.25(0.60)+0.20(0.50)=0.54.
$$

Do not multiply a dBm value or an unscaled delay directly into this normalized teaching score. Specify normalization bounds, clipping, uncertainty, and missing-data rules before testing.
With illustrative thresholds $J_1=0.25$, $J_2=0.50$, and $J_3=0.75$, Eq. `eq:em_degraded_mode_decision` proposes the GNSS-denied-navigation category. This is a proposed response classification, not evidence that the aircraft can perform that navigation. Require an implemented and validated alternative positioning method, acceptable uncertainty, and the relevant mission constraints. A single weighted index also cannot determine which sensor failed: severe packet loss alone could raise $J$, so retain component-specific checks.
Likewise, do not assume return-to-home is safe when positioning is untrusted. Identify the operationally approved fallback and its preconditions rather than automatically executing a label. The safety monitor remains an independent gate over any AI- or risk-score-generated proposal.
HAPS dependency and recovery record. Document whether the degraded observation concerns the UAV air link, relay processing, backhaul, authentication service, or shared timing source. Identify common dependencies affecting multiple UAVs. Distinguish measured link latency, command age, navigation uncertainty, and confirmed controller state. Require a sustained interval of acceptable service and mission-state checks before recovery acceptance; a lower score alone is insufficient.
Verification checklist.
Define admissible actions using state, uncertainty, and boundary constraints.
Normalize electromagnetic inputs and preserve their physical units separately.
Check response preconditions rather than treating a score as control authority.
Identify shared HAPS, backhaul, timing, and authentication dependencies.
Preserve proposal, monitor, execution, and recovery evidence separately.
Assign override, escalation, and recovery-acceptance responsibilities.
Expected deliverable. A worked command-gating calculation, normalized risk assessment, conditional degraded-mode policy, and evidence-backed recovery plan with explicit limits on autonomy and HAPS-model coverage.
Practical Exercise 16.3: Ecosystem Trust, Ethical Sensing, and Reproducible Assurance
Objective. Assess UTM identity and authorization, component provenance, privacy exposure, and benchmark performance without allowing a composite score to override mandatory obligations or evidence gaps.
Worked situation. The same HAPS-supported emergency mission shares selected observation products with a response center. A UTM authorization record is uncertain, a replacement component has incomplete provenance, and imagery may contain identifiable bystanders. Determine what can proceed, what requires review, and what further evidence is necessary. Use Eqs. `eq:utm_trust_score`, `eq:component_trust_score`, `eq:privacy_risk_score`, and `eq:uav_security_benchmark_objective`.
Worked solution: UTM trust. For Eq. `eq:utm_trust_score`, use weights $(\lambda_1,\ldots,\lambda_5)=(0.25,0.25,0.20,0.20,0.10)$ and inputs $(I,A,H,C,V)=(1,0,0.90,0.95,0.10)$. Then

$$
\Gamma_i=0.25+0+0.18+0.19-0.01=0.61.
$$

Here $A=0$ represents missing or unsatisfied authorization for the assessed activity, not evidence of hostile intent. Even if a classroom trust threshold were 0.60, the weighted score cannot replace a mandatory authorization check. Verify the applicable authorization record and scope, retain uncertainty, and provide a review/correction path before an adverse automated decision. A transmitted identifier alone is not verified identity or authorization.
Worked solution: component assurance. For Eq. `eq:component_trust_score`, choose weights $(0.30,0.25,0.20,0.15,0.10)$ and component inputs 

$(V_c,P_c,U_c,T_c,K_c)=(0.80,0.40,1.00,0.80,0.30)$. The index is
$$
\Omega_c=0.24+0.10+0.20+0.12-0.03=0.63.
$$

The low provenance input warrants specific evidence collection, such as serial/lot records and custody documentation. Do not infer malicious hardware or firmware from the score. Verify update integrity using the implemented mechanism and require any essential acceptance checks regardless of the average. Record open vulnerabilities, affected versions, owners, corrective actions, and review decisions.
Worked solution: privacy assessment. The supplied privacy discussion does not fully define every symbol in Eq. `eq:privacy_risk_score`. Before calculation, declare a consistent local interpretation. For this exercise only, use $D_s$ as data sensitivity, $L_s$ as location specificity, $T_r$ as collection/retention exposure, $R_d$ as onward-disclosure exposure, and $M_p$ as verified mitigation effectiveness, all normalized to $[0,1]$. This is a declared classroom rubric, not an additional definition assumed from the source text.
With weights $(0.25,0.20,0.20,0.20,0.15)$ and inputs $(0.90,0.80,0.70,0.60,0.50)$,

$$
\mathcal{P}_{risk}=0.225+0.16+0.14+0.12-0.075=0.57.
$$
Propose a documented minimization plan: collect only mission-needed products, reduce unnecessary location precision where appropriate, restrict recipients, define retention, and audit access. Test whether any redaction preserves task utility and whether identifiers remain recoverable from other metadata. Do not claim formal differential privacy merely because imagery is blurred or noise is added.
The result is an ordinal/weighted decision-support index, not a probability of privacy harm or a legal-compliance decision. Subtractive scores can fall outside $[0,1]$ unless you define a range convention. Mandatory data-governance requirements cannot be traded away by a favorable mitigation score.
Worked solution: reproducible benchmark. Apply Eq. `eq:uav_security_benchmark_objective` only after defining all metric denominators and normalizing latency and energy, or assigning weights with appropriate units. For a synthetic comparison, let normalized metric vectors $(SR,CR,FPR,LAT,EC,REC)$ be $\mathbf{m}{base}=(0.85,0.02,0.08,0.40,0.50,0.70)$, $\mathbf{m}{new}=(0.90,0.01,0.04,0.30,0.55,0.85)$.  
Using positive coefficient magnitudes $(0.30,0.20,0.10,0.15,0.10,0.15)$ with the equation’s prescribed signs gives $\mathcal{J}{base}=0.255-0.004-0.008-0.060-0.050+0.105=0.238$, $\mathcal{J}{new}=0.270-0.002-0.004-0.045-0.055+0.1275=0.2915$.
The new system improves the declared index by 0.0535 while increasing normalized energy cost from 0.50 to 0.55. Report that tradeoff rather than declaring universal improvement. A nonzero collision or constraint-violation rate still requires independent investigation and the applicable mandatory acceptance decision; a high aggregate score does not establish safety or ethical acceptability.
Use matched mission assumptions, independent evaluation runs, versioned configurations and models, raw metric records, and declared analysis methods. Report variability, unsuccessful cases, missing evidence, and hardware versus simulation provenance. Record human review workload and approval records separately if they are not represented in the objective.
Ethical and accountable decision record. For each proposed ecosystem action, preserve the affected stakeholders, verified evidence, uncertainty, alternatives considered, authorization basis, decision owner, data recipients, retention rule, and review or correction mechanism. Do not equate missing connectivity or documentation with malicious conduct. Test whether scoring creates systematic disadvantage under poor coverage or incomplete records, and document how you handle such cases.

Verification checklist.

Define all score inputs, normalization rules, weights, and evidence sources.
Keep identity, authorization, component integrity, and privacy obligations as explicit gates.
Resolve undefined privacy symbols before making comparisons.
Measure benchmark costs and preserve denominators, exclusions, and uncertainty.
Report adverse outcomes and energy or latency tradeoffs alongside improvements.
Assign accountable reviewers and provide a correction path for disputed evidence.
Expected deliverable. An ecosystem-assurance register linking the four equation-based assessments to mandatory checks, stakeholder impacts, reproducible benchmark evidence, unresolved gaps, and accountable review decisions.
