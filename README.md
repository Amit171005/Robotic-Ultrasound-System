# Robotic System for Abdominal Ultrasound Scanning

This B.Tech project explores whether an ultrasound probe can follow a curved abdominal surface and measure its shape by touch. The design pairs a four-motor Cartesian gantry with a passive wrist that tilts about the center of the probe face. A tilt sensor on the wrist supplies a local slope estimate for the proposed scan planner.

**Author** · Amit Kumar, Department of Mechanical Engineering, IIT Guwahati  
**Supervisor** · Prof. S. K. Dwivedy  
**Project status** · Phase I covered mechanical design, Fusion CAD motion and interference checks, actuator sizing, and analytical performance estimates. Fabrication, ROS 2 implementation, and gelatin-phantom tests are planned for Phase II.

## Why a gantry and a passive wrist?

The four-joint arm originally available in the lab needed 13.6 N·m of shoulder torque in the evaluated pose, against about 8.4 N·m available. It also lacked sideways probe tilt. A Cartesian gantry transfers the structural loads to linear guides and gives the probe three axes of translation.

An early gantry design used three wrist servos. Its tilt center sat 85 mm above the probe face, so an 18° tilt shifted the face sideways by about 26 mm. In the final design, two perpendicular circular guides share a center at the probe face. The bearings and guides sit above the surface, but the probe rotates about that virtual center. Ideally, the center of the face stays at the same point as the probe tilts.

## How the scanner is designed to work

| Assembly | Function |
| --- | --- |
| Gantry | Belt drives move X and Y. Two X motors home independently to square the bridge. A lead screw drives Z. Together the three translation axes use four stepper motors. |
| Self-aligning wrist | When one edge of the probe touches first, the contact force creates a moment that tilts the probe. Balance springs return the wrist toward upright when the gantry lifts it. |
| Force stage | A spring-loaded slider above the wrist accommodates vertical displacement. A load cell measures spring reaction for estimating contact force and providing Z-axis feedback. |
| Touch mapping | Gantry position and spring compression locate each contact point. An inertial measurement unit (IMU) measures the settled probe angle to estimate the local surface slope. |

## Phase I design results

| Quantity | Design value or calculated result |
| --- | --- |
| X × Y × Z travel | 580 × 500 × 250 mm |
| Passive wrist tilt | ±40° about Y and ±25° about X |
| Target contact-force range | 5–20 N |
| Typical predicted probe-face error | 0.83 mm in X and 0.59 mm in Y at 10 N, with 20° across-surface and 10° along-surface tilt |
| Worst analyzed probe-face error | 2.57 mm in X and 1.96 mm in Y at 20 N and the wrist’s tilt limits |
| Estimated parts cost | ₹78,245, including the stands and phantom materials. The estimate excludes the ultrasound machine, laptop, tools, and workshop time. |

Bending of the 6 mm guide rods in the force stage dominates the predicted positioning error. The wrist also introduces a separate uncertainty in the touch map. With an 18 mm probe footprint, the model predicts that the narrow-axis stage follows only about 43% of the surface slope under the assumed contact conditions. The IMU reads this settled angle, which makes calibration against known slopes an important part of the experiment.

## Planned scan and validation

1. Home the axes, zero the load cell, and establish the IMU reference on a level surface.
2. Touch a grid of points on a gelatin phantom. Record position, probe tilt, and force at each point.
3. Fit a surface in ROS 2 and plan overlapping back-and-forth sweeps within the wrist’s tilt range.
4. Follow the sweeps with Z-axis force feedback and record each ultrasound frame with probe position and force.

Bench tests will calibrate the force stage and verify the overtravel cutoff. Phantom experiments will measure angular tracking in both wrist stages, touch-map accuracy, force variation, and scan coverage.

## Project documents

- `BTP_Report_Overleaf.pdf` contains the load cases, design calculations, error budget, and cost breakdown.
- `BTP_Phase1_Presentation.pptx` summarizes the design evolution, CAD motion study, and Phase I results.
- `Self_Aligning_Sensing_Wrist_Concept_Guide.pdf` illustrates the virtual pivot, spring placement, and touch-mapping concept.
