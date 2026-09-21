# KOSCO 2026 Fall Tutorial — Day 3

![OpenFOAM](https://img.shields.io/badge/OpenFOAM-12-1f6feb)

OpenFOAM tutorial cases prepared for **Day 3 of the KOSCO 2026 Fall Tutorial**.

The repository contains three CFD/combustion examples arranged from a basic incompressible-flow case to reacting-flow simulations with detailed chemistry and transport.

---

## Tutorial cases

| Case | Description | Solver | Main features |
|---|---|---|---|
| [`1.cavity`](./1.cavity) | 2-D lid-driven cavity | `incompressibleFluid` | Incompressible Navier–Stokes, moving-wall boundary condition |
| [`2.SandiaFlameD`](./2.SandiaFlameD) | Sandia Flame D | `multicomponentFluid` | Methane/air non-premixed flame, detailed chemistry, GRI mechanism, axisymmetric wedge mesh |
| [`3.counterFlowFlame`](./3.counterFlowFlame) | H₂/air counterflow flame | `DTLreactingFoam` | Detailed chemistry, detailed transport, opposed-flow flame configuration |

---

## Requirements

The tutorial cases are based on **OpenFOAM Foundation v12**.

Make sure the OpenFOAM environment is loaded before running the cases:

```bash
foamVersion
```

The first two cases use solvers and models available in the standard OpenFOAM 12 environment.

> **Note**
>
> `3.counterFlowFlame` uses the custom `DTLreactingFoam` solver together with the detailed-transport models used in this tutorial.
> A standard OpenFOAM 12 installation alone is therefore not sufficient to run Case 3.
>
> Please install `DTLreactingFoam-12` before running this case:
>
> https://github.com/danhnam11/DTLreactingFoam-12

---

## Download

The recommended location is your OpenFOAM run directory:

```bash
cd $FOAM_RUN
git clone https://github.com/jjkimCombustion/KOSCO_2026_Fall_Tutorial.git
cd KOSCO_2026_Fall_Tutorial
```

To update an existing copy:

```bash
git pull
```

---

# 1. Lid-Driven Cavity

[`1.cavity`](./1.cavity) is a compact introductory case for reviewing the basic structure of an OpenFOAM simulation.

The domain is a **2-D square cavity**. The upper wall moves in the positive x-direction while the remaining walls are stationary.

The simulation is performed using standard `incompressibleFluid` module in OpenFOAM-12.

### Run

```bash
cd 1.cavity

blockMesh
foamRun
```

# 2. Sandia Flame D

[`2.SandiaFlameD`](./2.SandiaFlameD) introduces a reacting-flow calculation based on the well-known **Sandia Flame D** configuration.

The case uses an axisymmetric wedge mesh with separate methane-fuel, pilot, and coflow-air inlets.

The simulation is performed using standard `multicomponentFluid` module in OpenFOAM-12.

### Run

```bash
cd 2.SandiaFlameD

blockMesh
foamRun
```

For parallel execution, use the supplied decomposition settings:

```bash
decomposePar
mpirun -np 4 foamRun -parallel
reconstructPar
```

# 3. H₂/Air Counterflow Flame

[`3.counterFlowFlame`](./3.counterFlowFlame) is an opposed-flow reacting case in which a hydrogen-containing fuel stream and an air stream enter from opposite sides of the domain.

The supplied inlet velocities are:
```text
fuel : 0.5 m/s
air  : 0.5 m/s
```
Both inlet temperatures are initialized at `300 K`. A hot internal field is supplied to initialize the reacting region.

The fuel-side hydrogen mass fraction is:
```text
Y_H2 = 0.0673
```

while the air-side oxygen mass fraction is:
```text
Y_O2 = 0.23
```

The simulation is performed using custom `DTLreactingFoam` solver.

### Run

```bash
cd 3.counterFlowFlame

blockMesh
foamRun
```

Parallel execution can be performed in the same way:

```bash
decomposePar
mpirun -np 4 foamRun -parallel
reconstructPar
```

---

## Notes for the tutorial

These cases are intentionally compact so that the important OpenFOAM settings can be inspected directly.

Rather than treating the case files as black boxes, participants are encouraged to compare:

- mesh and boundary-condition definitions,
- incompressible and reacting-flow solver structures,
- thermophysical and chemistry models,
- numerical schemes and timestep settings,
- serial and MPI execution workflows.

The three cases are intended to provide a gradual path from a basic OpenFOAM CFD calculation to detailed reacting-flow simulations.
