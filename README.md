# Robotic-Ultrasound-System
Autonomous Robotic Ultrasound Scanner

B.Tech Project (BTP), Department of Mechanical Engineering, IIT Guwahati Author: Amit Kumar · Supervisor: Prof. S. K. Dwivedy

A low-DOF robotic manipulator that positions an ultrasound probe and scans a phantom on its own: it finds the phantom, plans a coverage sweep, keeps the probe normal to the surface and regulates the contact force, with no operator in the loop.

The project builds on an existing teleoperated ultrasound arm developed in an earlier PhD project at IIT Guwahati. The first phase of this BTP is a full engineering review of that arm, judged against the new goal of autonomous scanning, and a costed set of modifications that make autonomy feasible.

Status
Phase	State
Design review of the existing arm (statics, accuracy, workspace, orientation)	✅ Done (revision B, Oct 2026)
Component selection and cost estimate	✅ Done (revision B)
Hardware modifications (Option A)	⏳ Planned
Calibration, perception and force control	⏳ Planned
Autonomous path planning and coverage scanning	⏳ Planned
The existing arm
4 actuated joints (the original report calls it 5-DOF):
Axis 1 – base yaw: NEMA-42 stepper, direct drive, SKF 51209 thrust bearing
Axis 2 – shoulder: NEMA-42 stepper, direct drive, 2 × SKF 16002
Axes 3–4 – elbow / wrist: DS5180 hobby servos, direct drive
Measured joint spacings (drawing sheet 11): shoulder–elbow 379 mm, elbow–wrist 372 mm, wrist to probe-axis bend 328.8 mm, bend to probe tip 207 mm; shoulder axis 275 mm above the base.
Overall envelope: 901 × 445.5 × 765 mm.
Key findings

All numbers are from revision B of the calculations (realistic link masses, 0.80 kg end-effector).

Check	As designed	With Option A
Holding torque needed, shoulder (drawn pose / links horizontal)	13.6 / 25.0 N·m vs ≈8.4 N·m usable	Covered (geared NEMA-34 + brake)
Scan zone holdable (r 400–750 mm, z 100–300 mm, probe vertical)	0 %	100 %
Base tilting moment vs thrust-bearing capacity	18.7–28 N·m vs 3.9 N·m	Crossed-roller bearing
Probe-tip error (RSS)	13.0 mm	5.0 mm open loop, 1.7 mm with joint-encoder loops

Autonomy-specific checks

The probe contact force that cancels gravity torque (≈7–20 N at the shoulder) falls inside the 5–20 N scanning range, so the gear play is crossed while scanning → position loops on output-side joint encoders are required.
With the robot beside the phantom, 96 % of the upper surface needs ≤10° of sideways tilt, which these joints cannot produce. The probe is exactly normal on 51 % of the surface and within 15° on 71 %. Placing the robot at the phantom's end drops this to 21 % within 10°.
Three load cells on a Ø50 mm circle can estimate both contact force and contact offset (a 3 mm offset at 10 N shifts the cell readings by 0.8 N).

Verdict: the arm as designed cannot perform the autonomous scanning task. With Option A and the autonomy additions it is feasible at phantom level: phantom localisation, coverage sweeps with in-plane normal alignment, ±1 N force regulation and ≈1.7 mm tip error.

Option A – recommended modifications
#	Change
A1	Base: RU66 crossed-roller bearing + geared NEMA-23 (50:1) + flexible coupling
A2	Shoulder: NEMA-34 (8.5 N·m) + 10:1 planetary + 24 V brake (output limited ≈38 N·m)
A3	EN19 shaft, k5/H7 fit, clamp hub
A4	3:1 HTD belt reduction at the elbow
A5	Three load cells over a 0.5–1 N/mm spring stage (force + two tilt moments) instead of a 6-axis F/T sensor
A6	AS5048A encoders on all four joints after the gearboxes, position loops, homing
A7	Servo PWM ≥330 Hz
A8	Bolted base
A9	Safety: brakes, hold-then-brake E-stop, 25 N force limit, watchdog
A10	Optional probe-roll servo
A11	Depth camera + hand-eye calibration, or touch-probing a known phantom (≈25 points, ≈4–5 min)
A12	USB video capture of the ultrasound image
Cost estimate

Prices as of 12–13 Oct 2026, excluding the ultrasound probe and PC.

Build	Estimate	Machined free of cost
As designed	₹1.65–4.08 lakh	₹1.56–3.63 lakh
Recommended (Option A + autonomy, incl. depth camera)	₹1.68–2.77 lakh	₹1.57–2.22 lakh (₹1.13–1.78 lakh without camera)
Lean (touch-probing, economy base gearbox + output encoder, AS5600, Teensy 4.0, MS-pipe housing)	₹0.86–1.62 lakh	₹0.77–1.20 lakh

The 6-axis F/T sensor alone in the original design costs ₹0.87–2.40 lakh landed. A ₹50,000 build is not reachable with this arm.

Assumptions: customs exchange rates of 2 Oct 2026 (US$ 96.95, € 110.50, CNY 14.55); import uplift 1.20 (institute) to 1.65 (personal courier); machining ₹300–1,000/h.

Repository contents
├── README.md
├── calc.py                                       # all design calculations (statics, workspace, error budget, orientation)
├── docs/
│   ├── arm-design-review-findings.md             # summary of the review
│   ├── Robotic_US_Arm_Design_Calculations.pdf    # full calculation report
│   └── Robotic_US_Arm_Components_and_Feasibility.pdf
└── cost/
    └── Robotic_US_Arm_Cost_Estimate.xlsx         # itemised costs for all three builds
Running the calculations
bash
python calc.py
Roadmap
Build and assemble Option A (or the lean variant).
Joint encoders, homing and closed-loop joint control.
Phantom localisation (depth camera + hand-eye calibration, or touch-probing).
Three-load-cell force sensing and ±1 N contact-force regulation.
Coverage path planning over the phantom surface with in-plane normal alignment.
Synchronised ultrasound image capture along the scan path.
