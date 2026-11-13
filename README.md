# Sub-GHz Security Research

### RF Signal Analysis | Rolling-Code Authentication | Wireless Security Testing

A hands-on cybersecurity research project exploring Sub-GHz radio communications, rolling-code authentication, and replay resistance using a Flipper Zero and HaleHound.

This repository documents controlled experiments with personally owned RF devices, including a Genie Intellicode garage door opener and inexpensive wireless lighting controllers.

The objective is to understand how RF authentication systems operate, evaluate their observable security properties, and distinguish between successful signal capture and successful authentication.

## 1. Research Objectives

- Analyze Sub-GHz radio transmissions and modulation characteristics.
- Examine the differences between fixed-code and rolling-code authentication.
- Evaluate basic replay resistance in a controlled environment.
- Compare observed behavior with published RF security research.
- Document repeatable methodology, technical findings, and security implications.

All active testing is limited to personally owned or explicitly authorized equipment.

## 2. Hardware and Software

| Component | Description |
|---|---|
| Flipper Zero | Unleashed firmware; Sub-GHz Read, Read RAW, and frequency analysis |
| HaleHound | Sub-GHz spectrum analysis and RF testing |
| Garage door opener | Genie Professional Line Model 2028 |
| Authentication system | Genie Intellicode rolling-code technology |
| Lighting controllers | Inexpensive Sub-GHz wireless remotes |
| Analysis workstation | Windows PC |

## 3. Case Study 001: Genie Intellicode

### Research Question

Can a previously accepted Genie Intellicode remote transmission be captured and successfully replayed using general-purpose Sub-GHz security tools?

### Background

The Genie Professional Line Model 2028 uses Intellicode rolling-code authentication.

Unlike a fixed-code remote, which repeatedly transmits the same authorization value, a rolling-code transmitter changes its authentication value during successive activations.

The receiver tracks valid authentication state and normally rejects previously accepted codes.

This design mitigates straightforward capture-and-replay attacks.

### Experimental Setup

**Equipment:**
- Genie Professional Line Model 2028 garage door opener
- Authorized Genie remote
- Flipper Zero
- HaleHound

**Observed RF configuration:**
- Recording frequency: 390.00 MHz
- Flipper receiver preset: AM650
- Capture format: Sub-GHz RAW
- Number of remote activations: 3

The HaleHound spectrum analyzer also indicated RF activity in the 315 MHz and 390 MHz frequency ranges.

These observations were made using personally owned equipment.

### Methodology

1. Examined RF activity from the authorized garage door remote using the HaleHound spectrum analyzer.
2. Configured the Flipper Zero for Sub-GHz reception at 390 MHz using AM650.
3. Attempted protocol identification using the Flipper's standard Read function.
4. Found that the standard decoder did not produce a recognized, savable protocol entry.
5. Used Read RAW to record three authorized remote activations in a single capture.
6. Observed the garage receiver successfully respond to the original remote activations.
7. Separately attempted basic replay of previously accepted transmissions.
8. Observed that the replay attempts did not activate the garage door.

The experiment did not involve RF jamming, brute-force authentication, receiver modification, or unauthorized remote enrollment.

### Capture Observations

The RAW recording contained approximately 9.63 seconds of RF activity, including three apparent transmission groups corresponding to the three remote activations.

The Flipper displayed similar overall signal shapes between activations, with some visible differences.

These differences may result from changing encoded data, transmission duration, RF conditions, or other capture characteristics.

**The visual waveform alone does not identify the rolling counter or prove which encoded bits changed.**

Although the Flipper captured RF activity, its standard Read function did not identify a supported Genie protocol in this configuration.

This illustrates the distinction between:

- **Reception:** Detecting RF transmissions.
- **Recording:** Preserving signal timing information.
- **Decoding:** Interpreting protocol-specific fields.
- **Authentication:** Determining whether a receiver accepts a transmitted command.

### Results

| Test | Observation |
|---|---|
| Sub-GHz RF detection | Successful |
| RAW transmission capture | Successful |
| Standard protocol identification | Unsuccessful |
| Original remote operation | Successful |
| Basic replay of accepted transmissions | Unsuccessful |
| Rolling-code extraction | Not performed |
| Future-code prediction | Not demonstrated |

### Interpretation

The observed replay failure is consistent with Intellicode rolling-code replay protection.

However, this experiment does not prove that the Genie 2028 is immune to all replay, synchronization, or protocol-specific attacks.

The recording has not been fully decoded, and additional testing would be required to distinguish receiver rejection from potential limitations in signal reproduction.

**Finding:** Under the tested conditions, straightforward replay attempts did not reproduce an authorized garage-door activation.

## 4. Protocol Comparison: Genie Intellicode vs. Security+ 2.0

Chamberlain/LiftMaster Security+ 2.0 and Genie Intellicode are separate rolling-code authentication technologies.

Public research and software implementations exist for Security+ 2.0, including protocol encoders and decoders.

Specialized research tools also exist for certain Genie protocols, including applications that collect transmissions from an already authorized remote.

These techniques have different access requirements and should not be treated as equivalent attacks.

| Property | Genie Intellicode | Security+ 2.0 |
|---|---|---|
| Rolling-code authentication | Yes | Yes |
| Public protocol research | Available | Available |
| Standard Flipper decoding in this experiment | Not recognized | Not tested |
| Direct experimental validation | Basic replay resistance observed | Not performed |
| Single-capture cloning of tested system | Not demonstrated | Not evaluated |
| Specialized research tools | Available for certain Genie variants | Available |

### Security Considerations

A technique requiring prolonged physical access to an authorized remote presents a different practical threat from one requiring only brief observation of a radio transmission.

Attack feasibility depends on factors including:

- Access to authorized transmitters
- Protocol implementation
- Authentication state management
- Receiver synchronization behavior
- Available hardware and software
- Whether relevant vulnerabilities have been independently demonstrated

The availability of public encoding or decoding software does not, by itself, establish that arbitrary enrolled remotes can be cloned.

This project does not claim that either manufacturer is universally more secure.

## 5. Limitations

This is an exploratory hardware security experiment, not a comprehensive product security assessment.

Current limitations include:

- Only one garage opener and authorized remote configuration tested.
- RAW captures have not been fully decoded into protocol fields.
- Replay failures do not independently establish why the receiver rejected the transmission.
- No direct Security+ 2.0 testing was performed.
- No protocol-specific vulnerability was confirmed.
- Findings should not be generalized to all Genie or Chamberlain/LiftMaster products.

## 6. Future Research

Potential future work includes:

- Comparing pulse timing and framing across authorized remote activations.
- Improving the reliability of passive RF captures.
- Identifying modulation and encoding characteristics.
- Investigating publicly documented rolling-code designs.
- Comparing fixed-code lighting remotes with rolling-code systems.
- Developing scripts to visualize sanitized RF timing data.
- Documenting defensive measures against replay and unauthorized remote enrollment.

## 7. References

**Genie Company**
- [Official Genie website](https://www.geniecompany.com/)
- [Genie Model 2028](https://www.geniecompany.com/garage-door-opener-/model-2028)

**Flipper Zero**
- [Official Flipper Zero documentation](https://docs.flipper.net/)
- [Genie Door Recorder application](https://lab.flipper.net/apps/genie_record)

**Security+ Research**
- [argilo/secplus — Security+ and Security+ 2.0 protocol research](https://github.com/argilo/secplus)

## 8. Responsible Research

All testing is conducted on personally owned or explicitly authorized devices.

Raw garage-door authentication captures are not published because they may contain operational access credentials.

This repository focuses on experimental methodology, sanitized observations, defensive analysis, and reproducible research documentation.

---

**Project status:** Initial RF capture and replay-resistance observations completed. Further signal analysis and protocol comparisons are planned.
