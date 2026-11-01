# Public Datasets, Testbeds, Simulation Environments, and Analysis Tools
Reproducible evaluation requires enough information to reconstruct dataset preparation, the software environment, hardware configuration, and measurement procedure. Dataset documentation should describe provenance, labeling, collection conditions, limitations, and permitted use. Training, validation, and test partitions should avoid inappropriate overlap between related observations. Computational benchmarks should specify processor or accelerator type, available memory, software versions, workload, and timing methodology. Comparisons should use consistent conditions and distinguish offline processing from real-time operation. Versioned configurations, experiment identifiers, retained outputs, and documented changes support accountability.

## Practical Example: Building a Reproducible Scenario Benchmark
A reusable scenario collection requires more than configuration files. Reproducibility depends on preserving configuration identity, verifying application support, documenting execution conditions, and retaining observed results separately from expected behavior. This exercise describes how to organize an intended collection of 18 JSON files into a benchmark with explicit limitations. Inventory and evaluate the scenario files separately; this README does not report completed validation or execution results.

### Practical Exercise 12.1: Scenario Collection, Compatibility, and Reproducible Execution
**Objective:** Create a benchmark manifest for all 18 supplied JSON scenarios, verify compatibility with a specified simulator version, and define repeatable execution and measurement procedures without inferring runtime performance from the configuration structure.

**Worked situation:** Use all 18 supplied JSON files to create a reproducible scenario collection. Establish the exact collection from the supplied files rather than reconstructing filenames from memory. Record each filename, declared schema version, node types, timer settings, selected output, and simulator version. Check application support for specialized blocks, including `link_evaluation`, `replay_packets`, `multi_tone_barrage`, and `evaluation_output`.

The exercise produces a manifest and execution procedure; it does not guarantee support for every scenario or that every block behaves as its name implies. Unsupported, partially supported, and externally configured functions must remain visible in the benchmark record.

Practical procedure:
- [ ] Inventory the supplied collection and confirm that it contains 18 intended scenario files. Record duplicate names, identical contents, missing files, or ambiguity before freezing the collection.
- [ ] Preserve the original bytes and assign each scenario a manifest identifier. Record a content digest and collection revision to distinguish files that share a name but differ in content.
- [ ] Parse each JSON file and record its declared schema version. If no version is declared, record “not declared” rather than inventing one. Check compatibility against the identified application version.
- [ ] Inventory node identifiers, exact node-type strings, connection endpoints, timer parameters, and selected outputs. Retain relevant parameter units and reference points where available.
- [ ] Verify that the application recognizes and executes each required block type. Record parsing, rendering, routing, and semantic-execution support separately where they differ.
- [ ] 
Execute the scenarios using a documented procedure and retain run-specific settings, outputs, warnings, errors, and externally supplied functions. Measure computational performance during execution and record the platform and profiling method. Separate load/validation time from simulation and rendering costs.

>Hint: A JSON file can parse successfully while containing unsupported node types, invalid connections, or parameters the application ignores. A displayed block does not prove it executes its intended function. Preserve exact type strings and distinguish native application behavior from extensions, adapters, and manual settings.

**Worked solution:** benchmark manifest. Create a collection-level record with the collection identifier, an inventory of 18 scenario identifiers, the simulator release or commit, the environment specification, the execution procedure revision, and known limitations. Create one scenario-level record per file with the fields below. Use explicit “not declared,” “not supported,” or “not observed” values where appropriate rather than silently omitting missing information.
File identity: scenario identifier, exact filename, relative path, content digest, and source or revision.

* **Schema:** declared schema version, validator or compatibility rule, validation status, and validation messages.
* **Graph inventory:** node identifiers and types, connection endpoints, parameter references, disconnected nodes, and intentional monitoring-only elements.
* **Timing:** all timer settings, units, index origin, boundary convention, repetition, and observed discrepancies between labels and parameters.
* **Output selection:** configured output identifier, selected branch, scope reference point, and any application override applied during loading or execution.
* **Randomization:** seed fields and their scope, stochastic settings, application defaults, and whether replay of the same random realization is supported.
* **Compatibility:** support status for each required type, dependencies, external adapters, and unsupported or ignored parameters.
* **Execution and evidence:** procedure reference, run identifiers, expected behavior, observed results, logs, warnings, and computational measurements. Keep expected behavior at scenario level and observed results in linked run records. This lets you rerun executions without overwriting earlier observations. Preserve any modified configuration as a separate version linked to its original rather than replacing the source evidence.
* **Worked solution:** specialized-block support. For `link_evaluation`, verify the implemented metric definitions, input interpretation, and reference points. For `replay_packets`, verify packet identity, age, event timing, and probability semantics where supported. For `multi_tone_barrage`, verify the implemented waveform composition, power normalization, and timing. For `evaluation_output`, verify which branch and quantities are actually emitted or displayed. These are checks to perform, not assertions that the supplied application implements those functions. Assign a documented status such as native support, extension-dependent support, partial support, unsupported, or unverified. An application that only displays an unknown block has not established semantic support. Where practical, use small diagnostic inputs to confirm behavior independently of a complex scenario. Record required external packet validation, controller logic, detectors, measurement tools, or other functions separately from the JSON configuration.

**Connection validation:** Check that node identifiers are unique, that every referenced endpoint exists, that required inputs are satisfied, and that port or data-type rules are respected where the application defines them. Verify that the selected output is reachable through the intended active branch. Examine cycles according to application semantics rather than assuming all cycles are invalid. Record intentionally unconnected scopes or monitoring elements as such. Do not silently delete or reconnect nodes to make a file load.

**Repeatable execution instructions**
Start from the documented simulator and dependency versions and record the execution platform. Reset application state to the specified baseline. Load the manifest-selected file, verify its content digest, and capture validation and compatibility messages.
Confirm that loaded timer settings, selected output, and branch selection match the manifest. Record any automatic application changes or manual overrides. Apply only documented external settings and supported random seeds. Fix the number of frames or observation duration and the warm-up treatment. Run the scenario, preserve timestamped or frame-indexed outputs, and record whether execution completed, failed, or was only partially supported. Repeat as specified. When seeds are supported, distinguish same-seed repeatability checks from independent-seed variability trials. Compare observations with the declared expectations and tolerances. Record differences, missing evidence, and unresolved behavior without rewriting the expectations after seeing the results. Do not promise exact reproducibility when stochastic defaults, execution ordering, external state, or platform behavior are uncontrolled. State the level of repeatability demonstrated and the variables that remain uncontrolled.

**Computational performance:** Measure configuration loading and validation, scenario execution time, achieved processing rate, and peak memory use during actual execution. Record frame count, input size, active branches, selected output, rendering state, warm-up, repetition count, and the profiling tools used. Where useful, separate processing from graphical rendering and export operations. Report typical and tail timings, including platform, thread settings, and accelerator use where applicable.
JSON file size, node count, and connection count describe configuration structure; they do not establish execution latency, memory demand, or real-time capability. If a runtime measurement is unavailable, report it as not measured. Define the processing deadline before claiming real-time performance.

**Benchmark report:** Provide a collection summary showing all 18 files and their validation, compatibility, and execution status. Keep unsupported cases in the inventory and explain their effect on benchmark coverage. Report expected and observed behavior separately, preserve baseline observations, and link every conclusion to its run evidence. Report any aggregate result over supported cases with its denominator and exclusions so that incomplete support is not hidden.
Implementation checklist:

- [ ] Inventory and preserve all 18 intended configurations with exact filenames and digests.
- [ ] Record schema declarations, node types, timer settings, selected outputs, and simulator version.
- [ ] Validate connections and verify support for specialized-block execution.
- [ ] Record randomization settings and seed limitations where supported.
- [ ] Document unsupported, partially supported, and externally configured functions.
- [ ] Retain repeatable execution instructions and run-specific evidence.
- [ ] Distinguish expected behavior from observed results.
- [ ] Measure computational performance during execution rather than inferring it from JSON structure.

**Expected deliverable:** A benchmark manifest covering all 18 supplied JSON files, accompanied by repeatable execution instructions, compatibility records, preserved configurations, run evidence, and measured computational performance where available. Explicitly identify unsupported and externally configured functions.

