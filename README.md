# naca0015-active-flow-control-cfd
Validation of 2D steady-state active flow control (momentum injection) on a NACA 0015 airfoil against published literature.
Absolutely. I'll keep **all the content, numbers, findings, and technical meaning**, but convert the equations and notation into normal GitHub-readable text. No unnecessary academic formatting.

# 2D Active Flow Control (AFC) CFD Validation on NACA 0015 Airfoil

## Executive Summary

This repository contains a 2D steady-state CFD study of Active Flow Control (AFC) on a symmetrical NACA 0015 airfoil.

The study investigates continuous tangential blowing through a small fluidic actuator located near the leading edge. The simulations were carried out at a Reynolds number of 1.0 × 10^6.

Two cases were studied:

* **Baseline:** Actuator OFF, blowing velocity = 0 m/s
* **Actuated:** Actuator ON, continuous blowing velocity = 40 m/s

The Angle of Attack (AoA) was varied from 0° to 18° to study the effect of the actuator on lift, drag, flow separation, and stall behavior.

---

# Simulation Setup and Mesh

| Parameter                      | Value                                  | Purpose / Reference                          |
| ------------------------------ | -------------------------------------- | -------------------------------------------- |
| **Airfoil**                    | Symmetrical NACA 0015                  | Standard symmetrical airfoil benchmark       |
| **Chord (c)**                  | 0.3 m                                  | —                                            |
| **Actuator slot width**        | 1.0 mm (0.001 m)                       | Tangential leading-edge slot                 |
| **Actuator location**          | x/c = 0.10                             | Leading-edge flow control                    |
| **Domain**                     | 2D structured C-grid                   | —                                            |
| **Domain dimensions**          | 43.5c major, 10c minor, 30c wake       | Minimize far-field and wake boundary effects |
| **Surface nodes**              | 584                                    | Airfoil surface resolution                   |
| **Total cells**                | ~107,000                               | Computational mesh                           |
| **Total nodes**                | ~109,000                               | Computational mesh                           |
| **First cell height**          | 2.11 × 10^-4 c to 1.83 × 10^-5 c       | Boundary-layer resolution                    |
| **y+ range**                   | 0.8–9.2                                | Near-wall resolution                         |
| **Target y+**                  | ~1.0                                   | Enhanced Wall Treatment                      |
| **Turbulence model**           | k-epsilon with Enhanced Wall Treatment | Boundary-layer treatment                     |
| **Freestream velocity**        | 50 m/s                                 | Operating condition                          |
| **Reynolds number**            | 1.0 × 10^6                             | Operating condition                          |
| **Fluid model**                | Ideal Gas + Sutherland                 | —                                            |
| **Baseline actuator velocity** | 0 m/s                                  | Actuator OFF                                 |
| **Actuated velocity**          | 40 m/s                                 | Actuator ON                                  |
| **Actuation type**             | Continuous steady blowing              | Open-loop AFC                                |

The computational domain uses a structured 2D C-grid around the airfoil. The large far-field and wake regions were used to reduce the influence of the outer boundaries on the airfoil flow field.

The mesh contains approximately 107,000 cells and 109,000 nodes, with 584 nodes distributed along the airfoil surface.

The first cell height was varied from 2.11 × 10^-4 c to 1.83 × 10^-5 c. The resulting y+ values were between 0.8 and 9.2, with a target value of approximately 1.0 for resolving the near-wall flow.

---

# Coordinate System and Force Transformation

The CFD simulations were run by changing the direction of the freestream according to the Angle of Attack.

The freestream velocity components were defined as:

```text
Ux = U∞ × cos(AoA)

Uy = U∞ × sin(AoA)
```

Because the freestream direction changes with AoA, the raw force values from the CFD force monitors are measured relative to the global X-Y coordinate system.

Therefore, the raw force values cannot be directly treated as the airfoil's lift and drag.

The force components were transformed into the airfoil/body coordinate system using the following equations.

## Baseline — Actuator OFF

The baseline data are taken from **Tab A7**.

```text
True CL_OFF = P3 × cos(P4) - P2 × sin(P4)

True CD_OFF = P2 × cos(P4) + P3 × sin(P4)
```

Here, P4 is the Angle of Attack in degrees.

For the calculations, the angle is converted from degrees to radians:

```text
Angle in radians = P4 × π / 180
```

Therefore, the equations used in the actual calculation were:

```text
True CL_OFF =
P3 × cos(P4 × π / 180)
-
P2 × sin(P4 × π / 180)
```

```text
True CD_OFF =
P2 × cos(P4 × π / 180)
+
P3 × sin(P4 × π / 180)
```

---

## Actuated — Actuator ON

The actuated data are taken from **Tab B7**.

```text
True CL_ON = P7 × cos(P5) - P6 × sin(P5)

True CD_ON = P6 × cos(P5) + P7 × sin(P5)
```

Again, the Angle of Attack is converted from degrees to radians:

```text
Angle in radians = P5 × π / 180
```

The actual equations used were:

```text
True CL_ON =
P7 × cos(P5 × π / 180)
-
P6 × sin(P5 × π / 180)
```

```text
True CD_ON =
P6 × cos(P5 × π / 180)
+
P7 × sin(P5 × π / 180)
```

These transformations convert the global X-Y force components into lift and drag relative to the airfoil.

---

# Results and Validation

## 1. Lift Curve in the Linear Region

For AoA values below approximately 11°, the baseline airfoil shows an approximately linear increase in lift with increasing Angle of Attack.

The lift-curve slope obtained from the CFD results was:

```text
dCL/dAoA = 0.0890 per degree
```

The benchmark value used for comparison was:

```text
dCL/dAoA = 0.101 per degree
```

The benchmark is based on the MDPI Fluids 2020 study and Thin Airfoil Theory.

The CFD result is reasonably close to the benchmark. This indicates that the simulation is capturing the general behavior of the attached-flow region correctly.

The result also suggests that the mesh and near-wall treatment are able to resolve the boundary-layer development before the airfoil reaches stall.

---

# 2. Baseline Stall Behavior

The baseline NACA 0015 reaches its maximum lift at approximately:

```text
AoA = 15°–16°
```

The maximum lift coefficient obtained in this region was:

```text
CL_max = 1.1006–1.341
```

After approximately 16°, the lift starts to decrease significantly.

At 17°:

```text
CL = 1.002
```

At 18°:

```text
CL = 0.8584
```

The reduction in lift after stall is associated with increasing flow separation and trailing-edge separation, along with vortex shedding in the wake.

This baseline result provides the reference case for evaluating the effect of the active flow control system.

---

# 3. Effect of Continuous Blowing

The actuated case uses continuous 40 m/s blowing through a 1 mm wide slot located at:

```text
x/c = 0.10
```

The effect of the blowing changes considerably with Angle of Attack.

## Low Angle of Attack

At 0° AoA, the airfoil is geometrically symmetrical. Without actuation, the expected lift is therefore close to zero.

However, with continuous 40 m/s blowing, the simulation produced:

```text
CL = 0.374
```

The additional lift is caused by the interaction between the jet and the flow over the airfoil.

The tangential jet adds momentum to the flow near the surface and produces a Coanda-type effect. This changes the circulation around the airfoil and generates lift even at 0° AoA.

At low angles of attack, the actuator therefore behaves somewhat like a blown flap.

---

# 4. Behavior at High Angle of Attack

The effect of continuous blowing becomes different as the Angle of Attack increases.

At approximately:

```text
AoA ≥ 12°
```

the continuous 40 m/s jet begins to create an unfavorable displacement effect in the flow.

Instead of simply adding useful momentum to the boundary layer, the continuously injected jet creates a persistent region of high-momentum fluid near the leading edge.

At high angles of attack, this jet interacts with the incoming freestream and changes the local flow direction.

The jet effectively acts as a fluidic displacement barrier. Incoming streamlines are disturbed and displaced before reaching the airfoil surface.

This causes the flow to separate earlier rather than delaying separation.

In the present simulations, premature separation was observed around:

```text
AoA = 14°
```

Therefore, the continuous blowing configuration did not provide effective stall suppression at high angles of attack.

---

# 5. Lift Reduction at 16°

The difference between the baseline and actuated cases becomes particularly clear at 16°.

### Baseline

```text
CL_OFF = 1.1006
```

### Actuated

```text
CL_ON = 0.8708
```

The percentage change in lift is:

```text
Lift change = -20.88%
```

Therefore, at 16° AoA, continuous 40 m/s blowing reduced the lift coefficient by approximately 20.88% compared with the baseline case.

This is an important result because the actuator increases lift at low AoA but becomes detrimental near stall.

---

# Discussion

The CFD results show that the effectiveness of Active Flow Control depends strongly on both the actuator type and the Angle of Attack.

At low angles of attack, continuous tangential blowing can increase lift by adding momentum to the flow near the airfoil surface. The resulting Coanda effect increases circulation around the airfoil and produces additional lift.

However, the same continuous jet becomes less effective as the airfoil approaches stall.

At high AoA, the 40 m/s jet produces a continuous flow structure that interacts with the incoming freestream. Instead of only energizing the boundary layer, it creates a persistent disturbance in front of the airfoil surface.

This can cause the incoming flow to separate earlier.

The results therefore show that continuous steady blowing is not necessarily suitable for stall suppression at high angles of attack for this particular configuration.

---

# Continuous Blowing vs. Synthetic Jets

The behavior observed in this study is different from the expected behavior of a Zero-Net-Mass-Flux (ZNMF) Synthetic Jet Actuator (SJA).

A continuous blowing actuator continuously injects fluid into the flow. Because the injection is always present, the jet can create a persistent fluidic obstacle or displacement region.

A ZNMF synthetic jet operates differently. Instead of continuously adding mass to the external flow, it produces an oscillating jet through periodic blowing and suction.

The resulting unsteady flow can generate vortices that interact with the separated shear layer.

This provides a possible way of controlling separation without creating the same persistent fluidic barrier produced by continuous blowing.

Previous work considered in this validation study reports synthetic-jet operating frequencies in approximately the following range:

```text
250 Hz – 629 Hz
```

These frequencies could be investigated in future simulations to compare unsteady ZNMF actuation with the continuous blowing configuration studied here.

---

# Main Findings

The main findings from the CFD study are:

1. The baseline NACA 0015 produces a lift-curve slope of approximately **0.0890 per degree** in the linear region below approximately 11° AoA.

2. The benchmark lift-curve slope is approximately **0.101 per degree**, based on the MDPI Fluids 2020 study and Thin Airfoil Theory.

3. The baseline airfoil reaches stall at approximately **15°–16° AoA**.

4. The maximum baseline lift coefficient is in the range of **1.1006–1.341**.

5. After stall, the baseline lift decreases to **CL = 1.002 at 17°** and **CL = 0.8584 at 18°**.

6. Continuous **40 m/s** blowing through the **1 mm actuator slot at x/c = 0.10** generates additional lift at low AoA.

7. At **0° AoA**, the actuated case produces **CL = 0.374** due to the momentum added by the jet and the resulting Coanda effect.

8. At approximately **12° AoA and above**, the continuous jet begins to create an unfavorable fluidic displacement effect.

9. The actuated case shows premature separation around **14° AoA**.

10. At **16° AoA**, the baseline case produces **CL = 1.1006**, while the actuated case produces **CL = 0.8708**.

11. This corresponds to a **20.88% reduction in lift** with continuous blowing at 16° AoA.

12. Continuous steady blowing can therefore increase lift at low AoA but can become detrimental near stall.

13. The results suggest that unsteady ZNMF synthetic jets may be more suitable for high-AoA stall control because they can generate periodic disturbances and vortices without maintaining a continuous fluidic barrier.

---

# Repository Structure

```text
/
├── CAD/
│   ├── 2D airfoil surface geometry
│   └── Actuator slot definition files (.step)
│
├── Data/
│   └── Parametric sweep tables (.csv)
│       ├── Baseline / Actuator OFF
│       └── Actuated / Actuator ON
│
├── Plots/
│   ├── Flow-field velocity streamlines
│   ├── Static pressure contours
│   └── Validation and aerodynamic coefficient plots
│
└── Paper/
    └── IEEE conference-style paper preprint (.pdf)
```

## Folder Description

### `/CAD`

Contains the 2D NACA 0015 airfoil geometry and actuator slot definition files in `.step` format.

### `/Data`

Contains the CFD results from the AoA parametric sweep for both configurations:

* Baseline / actuator OFF
* Actuated / actuator ON

### `/Plots`

Contains the generated CFD visualization and validation results, including:

* Velocity streamlines
* Static pressure contours
* Lift and drag coefficient plots
* Validation plots

### `/Paper`

Contains the formal IEEE conference-style paper preprint describing the CFD methodology, validation, results, and conclusions in greater detail.
