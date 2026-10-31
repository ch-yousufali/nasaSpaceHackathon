# EVE-Med

## Product Requirements Document (PRD)

**Project type:** Hackathon prototype (fully simulated, no physical hardware)\
**Working title:** EVE-Med - Autonomous Onboard Medical Companion Robot for Space Missions\
**Version:** 2.0\
**Status:** Proposed product specification\
**Primary users:** Astronauts (crew), mission-control medical teams, flight surgeons, mission operators

---

## 1. Executive Summary

EVE-Med is a simulation-powered prototype of an onboard medical companion robot for astronauts. Most teams in this hackathon will build a vitals dashboard with alerts. EVE-Med is deliberately different: the product is **a robot character that lives with the crew, watches their health, talks with them, asks the right questions, and guides them through a pre-approved emergency protocol when no doctor is available**.

The robot is inspired visually by EVE from the film WALL-E: a smooth, white, egg-shaped floating robot with glowing eyes. It is a software-simulated robot rendered in the browser (3D or animated), paired with a simulated astronaut wearing a simulated smartwatch-style wearable.

Because the team has no budget for hardware, **everything is simulated**. The pitch is: the simulator produces data in the same format that real sensors would send, so real hardware can replace the simulation later if funding is won.

EVE-Med is a decision-support prototype, not a medical device. It does not diagnose, does not prescribe, and does not replace a qualified flight surgeon.

---

## 2. Problem Statement

On long missions such as a Mars transit, communication with Earth can be delayed by many minutes or interrupted entirely. If an astronaut becomes unwell, the crew may need to act before a flight surgeon can respond. Existing monitoring concepts mostly display numbers. They do not:

- Notice a problem and proactively talk to the astronaut.
- Collect symptoms that sensors cannot see (for example headache, dizziness, nausea).
- Combine sensor data with spoken symptoms.
- Guide a non-doctor crewmate step by step through an approved protocol.
- Escalate safely and clearly when the situation is beyond the robot's authority.
- Keep working when the link to mission control is down.

EVE-Med addresses all of these in a demonstrable, explainable way.

---

## 3. Product Vision

**"A caring, trustworthy medical companion that notices first, asks second, guides third, and escalates when it must."**

### Product principles

- **Sensors detect first, voice confirms second.** The robot notices changes in vitals on its own, then asks the astronaut how they feel.
- **Symptoms always count.** If the astronaut reports a symptom while vitals look normal, the robot still takes it seriously.
- **AI talks, rules decide.** The language model handles conversation only. A fixed rule engine decides which protocol applies.
- **Human in the loop.** The robot suggests; the human confirms. It never acts on medicines by itself.
- **Know your limits.** If a situation is not in the approved protocol library, the robot says so and escalates to mission control.
- **Transparency.** Simulated data, model outputs, and connection status are always clearly labelled.
- **Safety first.** Never claim a diagnosis. Never invent medical advice.

---

## 4. Goals and Non-Goals

### 4.1 Goals

- Simulate an astronaut with live vitals streamed from a simulated wearable.
- Simulate a floating companion robot (EVE-style) that reacts visually and by voice.
- Detect abnormal vitals using transparent rules plus an optional ML anomaly model.
- Collect symptoms through natural voice conversation using fixed triage questions.
- Combine vitals and symptoms to select an approved protocol.
- Show a virtual medical kit; the robot highlights an item and a human confirms.
- Escalate to mission control with evidence when needed.
- Continue basic local operation when mission-control connection is lost.
- Provide a scenario control panel so judges can trigger events in one or two clicks.
- Store readings, conversations, alerts, and events with an audit trail.

### 4.2 Non-Goals

The hackathon version will **not**:

- Connect to real astronaut medical equipment or real wearables.
- Claim NASA affiliation, approval, certification, or flight readiness.
- Provide diagnoses or treatment decisions made by AI.
- Let any AI model choose or recommend medicines on its own.
- Dispense real medication.
- Predict real medical events with clinically validated accuracy.
- Replace a flight surgeon or emergency protocol.

---

## 5. Target Users and Use Cases

### Primary user: Astronaut (crew member)

Wants a calm, reliable companion that watches their health, listens when they feel unwell, and tells them exactly what to do.

### Secondary user: Mission-control medical officer / flight surgeon

Needs clear evidence-based escalations: vitals, symptoms, protocol steps taken, and timeline.

### Secondary user: Mission operator

Needs to see system status, connection state, and data freshness.

### Core use cases

1. Routine end-of-day health check performed by the robot.
2. Robot notices abnormal vitals and starts a conversation.
3. Astronaut reports a symptom by voice and the robot runs a structured triage.
4. Robot guides the astronaut through an approved protocol.
5. Robot highlights a medical kit item for the astronaut to confirm and take.
6. Robot escalates to mission control with a full evidence package.
7. Communication blackout: robot keeps operating locally and syncs later.
8. Judge triggers scenarios from the control panel.

---

## 6. Roles in the Demo

This section removes confusion about who plays what.

- **The real person (you or a team member) plays the astronaut by voice.** You speak symptoms to the robot.
- **The simulated astronaut on screen is the body.** It carries the simulated wearable and produces the live vitals.
- **The simulated robot (EVE-style) is the caretaker.** It reads the wearable data, talks, asks questions, and guides.
- **The scenario control panel drives the vitals.** A judge or presenter clicks a scenario, and the simulated vitals change over time. Saying "my vitals are high" does not change the numbers; the panel does.
- **Voice drives the symptoms.** What the person says becomes structured symptom data.

This gives a clean rule: **the panel controls the body, the voice controls the symptoms.**

---

## 7. System Architecture

The system is split into clear layers so each part is simple, testable, and explainable.

### 7.1 Layers

1. **Simulation layer:** simulated astronaut, simulated wearable, scenario controller.
2. **Ingestion and processing layer (backend):** receives readings, validates them, stores them.
3. **Detection layer:** rule-based checks plus optional ML anomaly detection.
4. **Triage and protocol layer (rule engine):** combines vitals and symptoms and selects an approved protocol.
5. **Conversation layer (voice and language):** speech-to-text, language model for natural dialogue, text-to-speech.
6. **Safety guard layer:** final filter that blocks anything outside the approved library.
7. **Presentation layer:** 3D robot, astronaut view, medical kit, mission-control dashboard.

### 7.2 Data flow

Simulated wearable produces readings, which go to the backend. The detection layer evaluates them. If something is unusual, the robot is notified, turns toward the astronaut, and starts a conversation. The astronaut answers by voice. The conversation layer converts answers into structured symptom data. The rule engine matches vitals plus symptoms to a protocol. The safety guard checks the output. The robot reads the protocol aloud, highlights a kit item if one applies, and waits for human confirmation. If escalation criteria are met, an alert with evidence goes to mission control.

### 7.3 Key architecture rule

**The language model never decides the medical action.** It only phrases questions, understands answers, and reads out text that comes from the approved protocol library.

### 7.4 Connectivity simulation

- A connection toggle simulates the mission-control link being Connected, Degraded, or Disconnected.
- While disconnected, the robot keeps monitoring, detecting, and guiding using local logic.
- Events generated during the outage are queued.
- On reconnection, queued events are shown as synchronized.
- This demonstrates resilience; it is not proof of flight-grade fault tolerance.

---

## 8. Main Product Modules

### Module A - Simulated Astronaut and Wearable

A simulated astronaut wearing a simulated smartwatch-style wearable that streams vitals to the robot.

**Requirements:**

- Draw or render a watch on the astronaut's wrist with a visible data link to the robot.
- Generate heart rate, estimated blood oxygen (SpO2), and skin/wrist temperature.
- Optional: a visual-only synthetic ECG waveform.
- Normal variation (readings are never perfectly constant).
- Configurable update interval.
- Every reading and chart labelled **SIMULATED DATA**.
- Do not claim reliable blood pressure from the watch; leave it out of wearable claims. Respiratory rate may be shown as an estimate only.

**Acceptance criteria:**

- Values update automatically while the simulation runs.
- Scenarios change the relevant values smoothly over time, not in sudden jumps.
- Missing data is shown as missing and never treated as normal.

### Module B - EVE-Style Companion Robot

The visual and behavioural heart of the product.

**Design:**

- Smooth white egg-shaped body, floating, with glowing eyes (inspired by EVE from WALL-E; use an original design to avoid copying).
- Eye colour communicates state: calm (blue), attentive/scanning (cyan or white pulse), concern (amber), urgent (red).
- Gentle hover and turning animation toward the astronaut.
- A scan animation when checking vitals.
- Optional subtle on-body display or hologram panel showing what she sees.

**Behaviours:**

- Idle: floats beside the astronaut.
- Notice: turns toward the astronaut when something unusual is detected.
- Ask: speaks a question and listens.
- Guide: reads protocol steps one at a time and waits for confirmation.
- Highlight: points to or lights up a medical kit slot.
- Escalate: announces she is contacting mission control.

**Acceptance criteria:**

- The robot visibly reacts when a scenario starts.
- Eye colour and animation change with state.
- The robot never speaks medical content that is not from the approved library.

### Module C - Voice Conversation

**Requirements:**

- Speech-to-text to hear the astronaut.
- A language model used for natural phrasing and for turning spoken answers into structured data.
- Text-to-speech for the robot voice.
- Push-to-talk or wake phrase, plus a text fallback in case the microphone fails during judging.

**Notes:**

- Claude and ChatGPT models are language models, not voice systems by themselves. Pair a language model with separate speech-to-text and text-to-speech (browser speech APIs or hosted voice services).
- Keep a fallback path with no hosted API so the demo still works if the internet fails.

**Acceptance criteria:**

- The astronaut can report a symptom by speaking.
- The robot replies by voice with the next triage question.
- The conversation is logged in the timeline.

### Module D - Symptom Triage (Structured Questioning)

Sensors cannot see symptoms such as headache. The robot only knows if the astronaut tells it.

**How it works:**

- The astronaut says something like "I have a headache."
- The robot starts a fixed triage checklist for that symptom.
- Questions come from a checklist written by the team from public, reputable sources, not from AI imagination.
- The language model asks them naturally and converts answers into structured fields.

**Example triage fields for headache:**

- Location: left, right, front, back, all over.
- Intensity: 1 to 10.
- Duration.
- Onset: sudden or gradual.
- Associated symptoms: nausea, vision changes, dizziness, confusion, neck stiffness.

**Other symptom checklists to include (choose at least three for the demo):** headache, dizziness, nausea, shortness of breath, chest discomfort, fatigue, fever.

**Acceptance criteria:**

- Each symptom has its own checklist file.
- Answers are saved as structured data.
- Vitals are scanned at the same moment and combined with the answers.
- Symptoms reported with normal vitals are still processed.

### Module E - Detection Engine (Vitals Anomaly Detection)

Core detection must be transparent and testable. ML is an extra layer, never the only layer.

**Detection layers:**

1. Threshold rules on configured demonstration limits.
2. Rate-of-change rules over a time window.
3. Personal baseline comparison: deviation from that astronaut's own recent normal.
4. Multi-signal analysis: several vitals changing together.
5. Optional ML anomaly model (for example Isolation Forest) trained and tested on separate synthetic scenarios. Record features, report false positives and misses, and show a score only with an explanation of what it means.

**Acceptance criteria:**

- Identical input gives identical rule-based output.
- Each detection includes a reason and supporting measurements.
- Rule-based safety alerts work independently of the ML model.
- The system never claims a diagnosis.

### Module F - Protocol Library and Rule Engine

This is the safety backbone.

**Protocol library:**

- A small, fixed set of approved protocols written by the team from public sources (clearly cited in a sources file).
- Each protocol has: id, trigger conditions (vitals plus symptoms), ordered steps, kit item reference if any, recheck timing, escalation conditions, and a source reference.
- Protocols are stored as data files (for example JSON or YAML), not generated by AI.

**Rule engine:**

- Takes current vitals, detection results, and triage answers.
- Selects the matching protocol or decides there is no match.
- If there is no match, the robot says it cannot handle this and escalates to mission control.
- Escalation rules: severe symptoms, red-flag combinations, no improvement after a recheck, or unknown situation.

**Acceptance criteria:**

- The same inputs always select the same protocol.
- Every selected protocol cites its source in the UI.
- Unknown situations always escalate; the robot never improvises.

### Module G - Virtual Medical Kit

The robot's "hands" for the demo.

**Requirements:**

- A visual kit with labelled slots containing simulated items from the approved library only.
- The robot highlights one slot and states the protocol step, for example "take the item in slot three, as per protocol."
- The astronaut must confirm by voice or button before the step is marked done.
- Every action is logged with who confirmed and when.

**Hard rules:**

- The robot suggests and the human decides.
- No autonomous selection of medicines.
- No dosing logic made up by AI; any dosing text comes from the protocol file and its cited source.
- Everything is simulated; no real dispensing.

### Module H - Safety Guard

A final filter that checks every sentence the robot is about to speak or show.

**Requirements:**

- Allow-list of protocol ids, kit items, and phrasing templates.
- Block any mention of a drug or action not in the approved library.
- Block diagnostic claims (for example "you have X").
- On a block, replace the response with a safe fallback such as "I cannot advise on that. I am contacting mission control."
- Log every blocked response for the audit trail.

### Module I - Mission Control Dashboard and Alert Center

**Requirements:**

- Crew overview with status, latest vitals, data freshness, and connection state.
- Alert levels: Info, Warning, Critical.
- Each alert shows reason, vitals snapshot, reported symptoms, protocol used, and robot actions taken.
- Acknowledge and resolve actions with audit trail.
- Duplicate alert suppression for a configurable period.
- Persistent **SIMULATED DATA** indicator.

### Module J - Scenario Control Panel

The main live-demo tool for judges. No code editing required.

**Controls:**

- Select scenario.
- Start, pause, resume, reset.
- Set demonstration speed.
- Restore normal conditions.
- Toggle connection state.

**Initial scenarios (build at least three; five recommended):**

1. **Headache with warning signs:** astronaut reports a headache; triage escalates when severe pain plus vision changes plus abnormal vitals are reported.
2. **Gradual oxygen decline:** blood oxygen slowly drops; the system notices a trend before the critical threshold.
3. **Stress or exertion spike:** vitals rise; the robot notices, asks, runs a calm-down protocol, rechecks, and escalates if there is no improvement.
4. **Fever or temperature deviation:** temperature rises; the robot starts the matching protocol.
5. **Communication blackout:** link to mission control is lost while the robot continues locally and queues events.
6. **Recovery:** values return toward baseline and the event is resolved.

**Acceptance criteria:**

- A judge can trigger any scenario in one or two clicks.
- The robot, charts, detection engine, protocol engine, and alert center all respond to the same event.
- Reset returns everything to the normal state.

### Module K - Reports and Event Timeline

- Chronological timeline of readings, detections, conversations, protocol steps, confirmations, escalations, and connection changes.
- Filter by severity and event type.
- Downloadable report (CSV or PDF) with a synthetic-data disclaimer.

---

## 9. Example End-to-End Flow (Headache Scenario)

1. **Calm state:** EVE floats beside the simulated astronaut. Vitals are normal and labelled simulated.
2. **Astronaut speaks:** the person says, "I have a headache."
3. **Robot reacts:** eyes turn attentive, she turns toward the astronaut and says she will ask a few questions and check vitals at the same time.
4. **Triage questions:** where is the pain, how strong from 1 to 10, how long, sudden or gradual, any nausea, vision changes, or dizziness.
5. **Vitals scan:** scan animation plays; heart rate, blood oxygen, and temperature are read from the wearable.
6. **Rule engine decides:**
   - Mild pain with normal vitals leads to a rest-and-hydrate protocol with a recheck timer.
   - Severe pain with vision changes or abnormal vitals leads to immediate escalation.
7. **Robot guides:** reads the first step, waits for confirmation, continues step by step.
8. **Kit step (if the protocol includes one):** the slot lights up, the astronaut confirms, the action is logged.
9. **Recheck:** after the timer, vitals are scanned again and the astronaut is asked how they feel.
10. **Escalation if needed:** eyes turn red, EVE announces she is alerting mission control, and the alert appears with full evidence.
11. **Wrap-up:** event resolved, timeline saved.

---

## 10. AI and ML Strategy

### Roles

- **Language model:** conversation only (phrasing questions, understanding answers, reading approved text aloud). Not a medical authority.
- **ML model:** anomaly detection on vitals only (for example Isolation Forest on simulated normal and abnormal data).
- **Rule engine:** all action decisions.
- **Safety guard:** final check on every output.

### Why there is no medicine-choosing model

There is no trustworthy open-source model that safely decides medicines, and medical reviewers would reject one. Real systems use approved protocol libraries. The pitch line: **ML spots the problem, rules choose the action, the human confirms.**

### Implementation order

- **Phase 1:** deterministic detection and protocol engine.
- **Phase 2:** voice conversation and triage.
- **Phase 3:** optional ML anomaly model with documented evaluation.
- **Phase 4:** polish, summaries, reports.

---

## 11. Proposed Technology Stack

The stack is a proposal and should be adjusted to the team's skills and time.

| Layer | Proposed technology | Purpose |
| --- | --- | --- |
| Frontend | Next.js + TypeScript | Mission-control and astronaut UI |
| Styling | Tailwind CSS | Responsive dark interface |
| 3D robot | Three.js (or React Three Fiber); fallback: animated SVG/CSS | EVE-style robot |
| Charts | Recharts or similar | Vital trends |
| Backend | Python + FastAPI | Ingestion, detection, rule engine, APIs |
| Real-time | WebSockets | Push readings, alerts, robot state |
| Database | SQLite first, PostgreSQL if needed | Readings, alerts, events, conversations |
| ML | scikit-learn | Optional anomaly detection |
| Validation | Pydantic | Validate readings and structured symptom data |
| Speech | Browser speech APIs or hosted speech-to-text and text-to-speech | Voice in and out |
| Language model | Any hosted or local model, used for conversation only | Natural dialogue |
| Protocol data | JSON or YAML files with source references | Approved protocol library |
| Deployment | Cloud hosting or local demo | Host the demo |

**Fallback rule:** the demo must still work (with text input and browser voice) if a hosted API is unavailable.

---

## 12. Data Model (Initial)

### Astronaut

- `id`, `display_name`, `mission_id`, `status`, `created_at`

### WearableReading

- `id`, `astronaut_id`, `heart_rate`, `spo2`, `skin_temperature`, `respiratory_rate_estimate` (optional), `signal_quality`, `source` (must identify simulated data), `recorded_at`

### HealthBaseline

- `id`, `astronaut_id`, `metric`, `baseline_value`, `calculation_window`, `updated_at`

### SymptomReport

- `id`, `astronaut_id`, `symptom_type`, `answers` (structured JSON), `reported_at`, `triage_checklist_version`

### Protocol

- `id`, `title`, `trigger_conditions`, `steps`, `kit_item_ref`, `recheck_minutes`, `escalation_conditions`, `source_reference`, `version`

### ProtocolRun

- `id`, `astronaut_id`, `protocol_id`, `status`, `step_log` (each step with confirmation time and confirmer), `started_at`, `ended_at`

### Alert

- `id`, `astronaut_id`, `severity`, `title`, `reason`, `evidence` (vitals plus symptoms snapshot), `protocol_run_id`, `status`, `created_at`, `acknowledged_at`, `resolved_at`

### ConversationTurn

- `id`, `astronaut_id`, `speaker` (astronaut or robot), `text`, `blocked_by_guard` (boolean), `created_at`

### ScenarioEvent

- `id`, `astronaut_id`, `scenario_type`, `event_status`, `started_at`, `ended_at`, `configuration`

### ConnectivityEvent

- `id`, `state`, `changed_at`, `queued_event_count`

---

## 13. API Outline

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | `/api/astronauts` | List astronauts |
| GET | `/api/astronauts/{id}/readings` | Recent wearable readings |
| GET | `/api/astronauts/{id}/alerts` | Astronaut alerts |
| GET | `/api/alerts` | All alerts |
| POST | `/api/alerts/{id}/acknowledge` | Acknowledge alert |
| POST | `/api/alerts/{id}/resolve` | Resolve alert |
| POST | `/api/triage/start` | Start a symptom checklist |
| POST | `/api/triage/answer` | Submit a structured answer |
| GET | `/api/protocols` | List approved protocols |
| POST | `/api/protocol-runs/{id}/confirm-step` | Human confirms a step |
| POST | `/api/robot/say` | Send approved text through the safety guard to speech |
| POST | `/api/simulator/start` | Start simulated stream |
| POST | `/api/simulator/pause` | Pause stream |
| POST | `/api/simulator/scenarios` | Start a scenario |
| POST | `/api/simulator/reset` | Reset simulation |
| POST | `/api/simulator/connectivity` | Change connection state |

A WebSocket endpoint publishes new readings, robot state changes, alerts, protocol steps, and connectivity events to the frontend.

---

## 14. User Interface Requirements

### Main screens

1. **Astronaut Scene (hero screen):** the robot, the simulated astronaut with wrist wearable, a live vitals panel, the virtual medical kit, and a captions strip showing the conversation.
2. **Mission Control Overview:** crew status, active alerts, connection state.
3. **Alert Center:** prioritized alerts with evidence and actions.
4. **Scenario Panel:** scenario controls and live feedback.
5. **Event Timeline:** chronological history with filters.
6. **Protocol Library:** read-only view of approved protocols and their sources.

### Visual direction

- Dark, high-contrast, control-room feel; clean white robot as the visual focus.
- Restrained colour: neutral for normal, amber for warning, red for critical, soft blue or cyan for the robot's calm state.
- Clear typography and readable numbers; charts with labelled axes and units.
- Persistent **SIMULATED DATA** indicator on every screen.
- Clear connection and data-freshness indicators.
- Captions always visible so the demo works even with poor audio.
- Avoid decorative animation that hides urgent information.

---

## 15. Safety, Privacy, and Ethics

- Use only synthetic data; no real personal medical data.
- Do not present the app as NASA-approved, clinically validated, or suitable for real astronaut care.
- Do not expose API keys or secrets in frontend code.
- Validate all incoming readings and structured answers.
- Keep an audit trail of conversations, protocol steps, confirmations, blocked responses, and scenario actions.
- Protocol content must come from cited public sources and be clearly marked as demonstration content, not medical advice.
- Always show that the human confirms actions and that mission control can override.
- Clearly label unvalidated model outputs.

---

## 16. Hardware Story (Pitch Only, Not Built)

Because there is no budget for parts, hardware is shown as a **funded future roadmap**, backed by a simple diagram.

**Future hardware:**

- Wrist wearable (heart rate, blood oxygen, temperature, optional ECG).
- Robot body with camera, microphone, speaker, and small manipulator arm.
- Locked medical kit with a controlled, human-confirmed dispenser.
- Onboard computer running detection and protocol engine locally.
- Secure link to mission control.

**Honest notes for the pitch:**

- Consumer smartwatches measure heart rate well for everyday use using optical sensing, but they are not medical-grade and are not certified for spaceflight.
- Motion, strap fit, and skin factors affect accuracy.
- Reliable blood pressure on a watch is not a safe claim.
- Real deployment needs medical-grade sensors, calibration, signal-quality checks, secure communications, fault handling, clinical validation, and aerospace qualification.

**Key selling point:** the simulator already uses the same data format real sensors would send, so swapping in hardware later is straightforward.

---

## 17. Hackathon Implementation Plan

### Phase 1 - Foundation

- Repository and project structure.
- Simulated astronaut, wearable, and vitals generator.
- Basic robot on screen (even a simple animated version first).
- Dashboard shell.

**Deliverable:** robot and astronaut on screen with changing simulated vitals.

### Phase 2 - Detection and Protocols

- Threshold, trend, and baseline rules.
- Protocol library files and rule engine.
- Alert creation and history.

**Deliverable:** a changing vital triggers an explainable alert and selects a protocol.

### Phase 3 - Conversation and Triage

- Speech in and out (with text fallback).
- Triage checklists for at least three symptoms.
- Language model layer for natural dialogue and structured extraction.
- Safety guard.

**Deliverable:** the person speaks a symptom, the robot asks questions and guides a protocol.

### Phase 4 - Scenarios, Kit, and Escalation

- Scenario control panel.
- Virtual medical kit with human confirmation.
- Escalation to mission control with evidence.
- Connectivity blackout simulation.

**Deliverable:** a judge triggers an event and watches the whole chain.

### Phase 5 - Polish

- Robot animation and eye states.
- Event timeline and report export.
- Test reset, pause, reconnect, missing data, repeated alerts, microphone failure, and API failure.
- Prepare the scripted demo and pitch.

**Deliverable:** stable, hackathon-ready prototype and presentation.

---

## 18. Prioritization

### Must have

- Simulated astronaut with wearable vitals, clearly labelled.
- EVE-style robot on screen with at least basic state changes.
- Rule-based detection with explainable alerts.
- Protocol library and rule engine.
- Voice or text symptom triage for at least one symptom (headache).
- Safety guard.
- At least three working scenarios.
- Human confirmation for kit steps.
- Escalation with evidence.
- Reset and recovery.

### Should have

- Personal baselines.
- Multi-signal analysis.
- Virtual medical kit visuals.
- Connectivity blackout simulation.
- Event timeline.

### Could have

- Isolation Forest anomaly model with evaluation.
- Downloadable reports.
- Synthetic ECG visual.
- Authentication and roles.
- Multi-astronaut crew view.
- Digital twin visualization.

**Priority rule:** a working, explainable, end-to-end flow with a charming robot is worth more than many unfinished AI features.

---

## 19. Differentiation (How We Stand Apart)

Most teams will build a vitals dashboard with simulated sensors and alerts. EVE-Med differs because:

1. **Character-driven:** a caring robot with personality, not just charts.
2. **Two-way:** it listens to symptoms and asks structured questions, not just reads sensors.
3. **Safety by design:** AI talks, rules decide, humans confirm, unknown cases escalate.
4. **Built for the no-doctor situation:** the real space problem of delayed or lost communication.
5. **Honest and fundable:** simulation now, clear hardware roadmap, same data format.

---

## 20. Demo Storyboard (Three Minutes)

1. **0:00 - 0:20:** Introduce the problem: no doctor nearby on a long mission. Show EVE-Med scene with simulated astronaut and robot. State that all data is simulated.
2. **0:20 - 0:50:** Show the wearable link and live vitals; explain baseline and detection.
3. **0:50 - 1:40:** Presenter says, "I have a headache." Robot runs triage by voice, scans vitals, selects a protocol, guides the first steps, and highlights a kit slot; presenter confirms.
4. **1:40 - 2:15:** Presenter clicks a second scenario (gradual oxygen decline). Robot notices on its own, asks how the astronaut feels, then escalates with evidence to mission control.
5. **2:15 - 2:40:** Trigger communication blackout; show local operation and queued events, then reconnection and sync.
6. **2:40 - 3:00:** Show the hardware roadmap diagram and the safety statement: ML spots the problem, rules choose the action, the human confirms.

### Suggested pitch line

"On a long mission, help can be hours away. EVE-Med is a companion robot that notices first, asks the right questions, and guides the crew through approved protocols, and when it reaches its limits, it escalates to mission control. Today it runs on simulated data; tomorrow, the same software runs on real sensors."

Do not claim the prototype predicts or prevents real medical emergencies.

---

## 21. Success Criteria

The prototype is successful if:

- The simulated astronaut and wearable stream vitals without manual refresh.
- The robot visibly and audibly reacts to scenarios.
- A spoken symptom leads to structured triage and a protocol selected by rules.
- Every alert includes reason and evidence.
- The safety guard blocks content outside the approved library.
- At least three scenarios run from the control panel.
- The communication blackout does not stop local operation.
- The interface clearly distinguishes simulated data from real data.
- The team can explain the architecture, safety design, limitations, and future hardware path.

---

## 22. Risks and Mitigations

| Risk | Mitigation |
| --- | --- |
| Microphone or speech failure during judging | Text input fallback and always-visible captions |
| Hosted API or internet failure | Local fallback path for speech and dialogue templates |
| Too many modules for the time | Follow the must-have list first; cut could-haves |
| 3D robot takes too long | Start with an animated 2D or SVG robot, upgrade later |
| AI says something unsafe | Safety guard with allow-list and fallback response |
| Judges question medical validity | Clear disclaimers, cited protocol sources, human-in-the-loop design |
| Demo glitches | Scripted demo, reset button, rehearsed fallback scenario |

---

## 23. Open Questions

1. What is the hackathon's exact deadline and judging rubric?
2. Are there required technologies or restrictions?
3. How many team members, and who owns frontend, backend, robot animation, voice, and presentation?
4. Is a deployed public URL required?
5. Which public medical sources will be used for the protocol library?
6. Which three to five symptoms and scenarios will be in the final demo?
7. Which speech and language services are allowed and affordable?

---

## 24. Final Product Definition

EVE-Med is a **simulation-powered, safety-first companion robot for astronaut health**. Its defining experience is the complete chain:

**Simulated wearable vitals → robot notices → robot asks → astronaut answers by voice → rules select an approved protocol → human confirms each step → escalation to mission control with evidence → resilient local operation.**

This is the core product to build first. Every additional feature should strengthen this chain rather than distract from it.
