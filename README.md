# Bo-oho-wa-ah

**An integrated systems-engineering study connecting seawater-cooled data-centre infrastructure, thermal and water-stream recovery, electrolytic propellant production concepts, and reusable launch vehicle simulation.**

> **Status: Conceptual architecture and engineering investigation.** This README describes the intended system, its governing relationships, and its validation approach. No subsystem is documented here as implemented, validated, or operational. Unknown implementation details are shown as `[TBD]` placeholders. Quantitative values appear only where they are derived from stated equations and assumptions, and are labeled as such.

---

## Table of Contents

1. [Overview](#1-overview)
2. [The Engineering Challenge](#2-the-engineering-challenge)
3. [System Vision and Integrated Architecture](#3-system-vision-and-integrated-architecture)
4. [Seawater-Cooled Data-Centre Infrastructure](#4-seawater-cooled-data-centre-infrastructure)
5. [Thermal Integration and Water Processing](#5-thermal-integration-and-water-processing)
6. [Rocket Propellant Production](#6-rocket-propellant-production)
7. [Reusable Launch Vehicle Simulation](#7-reusable-launch-vehicle-simulation)
8. [System Models and Governing Equations](#8-system-models-and-governing-equations)
9. [Data Flow and Module Interfaces](#9-data-flow-and-module-interfaces)
10. [Installation and Usage](#10-installation-and-usage)
11. [Example Simulations and Visualizations](#11-example-simulations-and-visualizations)
12. [Validation and Quantitative Analysis](#12-validation-and-quantitative-analysis)
13. [Engineering Assumptions and Current Limitations](#13-engineering-assumptions-and-current-limitations)
14. [Environmental, Safety, and Feasibility Considerations](#14-environmental-safety-and-feasibility-considerations)
15. [Development Roadmap](#15-development-roadmap)
16. [References and Technical Resources](#16-references-and-technical-resources)
17. [Third-Party Acknowledgements](#17-third-party-acknowledgements)
18. [Contribution Guidelines](#18-contribution-guidelines)
19. [License](#19-license)

---

## 1. Overview

Boohowaer asks whether several engineering domains that are usually studied separately can be designed as one coherent system. The domains are:

- Data-centre thermal management.
- Seawater heat rejection and water-stream processing.
- Electrolytic production of hydrogen and oxygen.
- Simulation of reusable launch vehicles.

Each domain has its own physics, scale, and constraints. The project's contribution is to make the connections between them explicit: what each subsystem produces, what it requires, and where those requirements cannot be met by the others.

The central engineering question is:

> Can computing infrastructure, seawater-based cooling, resource processing, and propellant production be designed as a coherent system that supports the simulation and analysis of reusable launch vehicles?

The project treats this as an open investigation. Its answer may be that some connections are feasible, some are not, and some depend on external energy and infrastructure that the system does not supply.

## 2. The Engineering Challenge

The main difficulty is energy quality and quantity, not the availability of seawater or waste heat.

- A data centre rejects almost all of its electrical input as heat, but at low temperature. Heat at roughly 30–45 °C is useful for space heating or some low-temperature processes, and is of limited use for driving chemical reactions or generating electricity efficiently.
- Electrolysis of water requires electrical energy. The theoretical minimum for splitting water is set by the standard enthalpy and Gibbs energy of the reaction; practical systems consume more because of overpotentials, resistance, and auxiliary loads.
- Liquefaction of hydrogen and oxygen requires substantial additional energy, which is a larger share of the total than many early estimates assume.
- Rocket propulsion depends on propellant mass, which is constrained by the vehicle's structural and performance requirements.

The integrated system is therefore constrained mainly by the electrical and thermal energy it must import, and by the temperature at which waste heat is available. These constraints set the feasibility questions the project must answer.

## 3. System Vision and Integrated Architecture

The proposed system has five connected parts:

1. **Seawater-cooled computing infrastructure.** Heat from high-density compute is rejected to seawater through heat exchangers.
2. **Thermal and water-stream recovery.** Heated seawater and treatment outputs are characterized for usefulness.
3. **Propellant production.** Purified water is electrolysed into hydrogen and oxygen, which are separated, conditioned, and stored.
4. **Reusable launch vehicle simulation.** A physics-based model simulates ascent, flight, descent, and recovery using propellant parameters from the production model.
5. **Integrated analysis.** Material, energy, and data flows are tracked across the modules to test feasibility.

The architecture is described in [Section 9](#9-data-flow-and-module-interfaces) and illustrated in the diagram below. The diagram shows intended relationships only.

## 4. Seawater-Cooled Data-Centre Infrastructure

### 4.1 Scope

The cooling subsystem models the heat generated by a data centre and the seawater flow needed to reject it. Relevant quantities are:

- Heat load from IT equipment, in kW or MW `[TBD: design heat load]`.
- Seawater supply temperature, which varies with location and season `[TBD: site data]`.
- Allowable coolant temperature rise and maximum discharge temperature `[TBD: regulatory and equipment limits]`.
- Heat-exchanger effectiveness and thermal resistance `[TBD: exchanger design]`.
- Pumping power and pressure drop `[TBD: piping and pump specification]`.

### 4.2 Governing Relationship

The heat transferred to the seawater stream is:

$$\dot{Q} = \dot{m}\, c_p\, \Delta T$$

where:

- $\dot{Q}$ is heat transfer rate (W),
- $\dot{m}$ is seawater mass flow rate (kg/s),
- $c_p$ is specific heat capacity of seawater (J/(kg·K)), approximately 3,990 J/(kg·K) at typical surface salinity and temperature, to be confirmed for site conditions,
- $\Delta T$ is temperature rise of the seawater across the exchanger (K).

The relation assumes steady flow, negligible heat loss to surroundings, and constant $c_p$ over the temperature range. For a given heat load, a larger flow rate gives a smaller temperature rise, which reduces discharge temperature but increases pumping power.

### 4.3 Key Performance Quantities

| Quantity | Definition | Status |
|---|---|---|
| Pumping power | Hydraulic power to move seawater through the circuit | Calculated when pump and piping data are supplied |
| Heat-exchanger effectiveness | Actual heat transfer divided by maximum possible | `[TBD]` |
| Power Usage Effectiveness (PUE) | Total facility energy divided by IT equipment energy | `[TBD: measured or calculated, not yet available]` |
| Heat-reuse fraction | Recovered thermal energy divided by total rejected heat | `[TBD]` |

### 4.4 Engineering Concerns

- **Corrosion and materials.** Seawater is corrosive. Titanium, copper-nickel alloys, and duplex stainless steels are common choices, each with trade-offs in cost and fabrication `[TBD: materials selection]`.
- **Biofouling and scaling.** Biological growth and mineral deposits reduce heat-transfer coefficients and increase pressure drop. Mitigation includes filtration, chlorination or alternatives, and scheduled cleaning, each with environmental implications.
- **Intake and discharge.** Intake can entrain marine organisms. Discharge alters local water temperature. Both are subject to environmental permitting and must be modeled against site-specific limits.
- **Maintenance.** Access for cleaning, inspection, and repair affects system layout and downtime.

### 4.5 Energy Consumption

Cooling energy adds to the facility's total electrical demand through pumps, auxiliary equipment, and any active chillers. Seawater cooling can reduce chiller load compared with air cooling, but the net benefit depends on pumping power and site conditions. The efficiency of the cooling system should be evaluated against the total energy budget, not in isolation.

## 5. Thermal Integration and Water Processing

### 5.1 Heat-Exchanger Interfaces

Heat exchangers separate the seawater stream from process streams, preventing contamination and allowing each side to be controlled independently. Candidate configurations include plate and shell-and-tube exchangers. Their effectiveness and approach temperature determine how much heat can be transferred to a process at a given temperature `[TBD: design selection]`.

### 5.2 Seawater Pretreatment and Desalination

Electrolysis requires purified water. Seawater must be treated before use:

- **Pretreatment:** screening, filtration, and removal of suspended solids and organisms.
- **Desalination:** reverse osmosis is the most common route, consuming electrical energy at typical pressures of several megapascals for seawater. Thermal desalination is possible but requires heat at higher temperatures than typical data-centre waste heat `[TBD: route selection]`.
- **Brine management:** desalination produces concentrated brine, which must be discharged, diluted, or treated. Its volume and salinity depend on recovery ratio.

### 5.3 Water Recovery and Quality

Water recovery ratio, salinity of product and brine, and energy per unit of product water are the key outputs. Electrolysis typically requires water of high purity, and the required specification depends on the electrolyser technology `[TBD: electrolyser specification]`.

### 5.4 Usefulness of Waste Heat

Low-grade waste heat is limited in use. Its temperature sets which processes it can support:

| Heat temperature | Typical usable applications | Relevance to this project |
|---|---|---|
| Below ~40 °C | Space heating, preheating feedwater | Limited; does not drive electrolysis or liquefaction |
| ~40–80 °C | Low-temperature preheating, some desalination stages | Partial contribution to preheating |
| Above ~80 °C | Some thermal desalination and process heat | Usually not available from data-centre cooling loops |

Preheating feedwater can reduce electrical demand slightly, but it does not replace the electrical energy required for electrolysis. Heat is not interchangeable with electricity, and its value depends on the temperature at which it is delivered.

### 5.5 Thermal and Material Balances

The subsystems must balance:

- Heat rejected from compute equals heat absorbed by seawater, plus heat recovered to processes, plus losses.
- Water entering treatment equals product water plus brine plus losses.
- Each balance should be checked numerically, with residuals reported.

## 6. Rocket Propellant Production

### 6.1 Chemistry and Products

Electrolysis of water proceeds according to:

$$2\,\text{H}_2\text{O}(l) \rightarrow 2\,\text{H}_2(g) + \text{O}_2(g)$$

Water is not rocket fuel. Hydrogen and oxygen are separate products that serve as fuel and oxidizer respectively in some propulsion systems. Their stoichiometric mass ratio is approximately 1:8 by mass for hydrogen to oxygen, and the mass of propellant produced is limited by the mass of water consumed.

### 6.2 Energy Requirements

The theoretical electrical energy for water electrolysis is set by the standard reaction enthalpy. Practical electrolysers consume more because of overpotentials, ohmic losses, and balance-of-plant loads. Typical practical energy use for current commercial electrolysis is on the order of 50–55 kWh per kilogram of hydrogen `[TBD: confirm against cited reference for chosen technology]`. This electrical energy must be supplied from an external source. Waste heat from cooling does not substitute for it.

### 6.3 Processing Stages

Each stage contributes energy demand, equipment requirements, and losses:

- **Electrolysis:** conversion of purified water into hydrogen and oxygen, with electrical input.
- **Separation:** removal of residual water and gas separation where needed.
- **Drying:** removal of moisture before compression or storage.
- **Compression:** raising gas pressure for storage, with significant electrical demand.
- **Liquefaction:** cooling hydrogen and oxygen to cryogenic temperatures. Hydrogen liquefaction is energy-intensive and requires temperatures near 20 K. Oxygen liquefaction occurs near 90 K. Both demand energy beyond that of electrolysis `[TBD: energy estimate from cited source]`.
- **Storage:** cryogenic or compressed storage with boil-off management.

### 6.4 Mass and Production Balance

The propellant model tracks mass flow through each stage, with losses at each step. Production rates are outputs of the design, not assumptions, and are not stated here. They depend on the electrical power available, electrolyser size, and process efficiency `[TBD]`.

### 6.5 Role of Recovered Heat

Recovered data-centre heat can contribute to preheating water, to regeneration in some drying or separation steps, and to limited process heating. Its contribution is bounded by its temperature and by the energy demands of the process. It is not expected to supply electrolysis, compression, or liquefaction, which are electrically driven or require low-temperature refrigeration.

### 6.6 Process Safety

Hydrogen and oxygen together form an explosive mixture over a wide concentration range. Hydrogen is flammable and has low ignition energy, and cryogenic handling introduces embrittlement and asphyxiation hazards. Process design must include gas detection, ventilation, separation of oxygen and hydrogen streams, pressure relief, and material compatibility. These considerations are design requirements, not optional features.

## 7. Reusable Launch Vehicle Simulation

### 7.1 Purpose

The simulation subsystem is a physics-based model of a launch vehicle's flight. It is intended to explore the engineering problems that reusable launch systems face, including propellant budgeting, trajectory design, descent, and landing. It is an educational and analytical model, not a reproduction of any operational vehicle.

### 7.2 Modeling Areas

- **Mass and propellant.** Vehicle mass changes as propellant is consumed.
- **Propulsion.** Thrust and specific impulse determine acceleration and propellant consumption.
- **Gravity.** Acceleration varies with altitude.
- **Atmosphere and drag.** Density, drag coefficient, and reference area determine aerodynamic forces.
- **Trajectory integration.** Numerical integration of equations of motion, with stated step size and error tolerance.
- **Staging and coast.** Staging events and coast phases, if included, are modeled as discrete changes in mass and thrust.
- **Re-entry and descent.** Aerodynamic and propulsive deceleration, with thermal loads noted as outside scope unless modeled.
- **Landing.** Touchdown velocity and propellant reserve requirements.
- **Guidance.** Simplified guidance laws, documented as assumptions.

### 7.3 Governing Equations

Newton's second law for the vehicle along its trajectory:

$$m(t)\,\frac{d\mathbf{v}}{dt} = \mathbf{T} + \mathbf{D} + \mathbf{W}$$

where $m(t)$ is instantaneous vehicle mass (kg), $\mathbf{v}$ is velocity (m/s), $\mathbf{T}$ is thrust vector (N), $\mathbf{D}$ is aerodynamic drag (N), and $\mathbf{W}$ is gravitational force (N).

The ideal rocket equation relates velocity change to propellant mass:

$$\Delta v = I_{sp}\, g_0 \ln\!\left(\frac{m_0}{m_f}\right)$$

where $I_{sp}$ is specific impulse (s), $g_0$ is standard gravity (9.80665 m/s²), $m_0$ is initial mass (kg), and $m_f$ is final mass (kg). The equation assumes impulsive burns, no gravity or drag losses, and constant exhaust velocity. Real trajectories incur losses, so the equation provides an upper bound on $\Delta v$ for a given propellant fraction, not a flight estimate.

Drag force is:

$$D = \tfrac{1}{2}\,\rho\, v^2\, C_D\, A$$

where $\rho$ is atmospheric density (kg/m³), $C_D$ is drag coefficient, and $A$ is reference area (m²).

### 7.4 Simplified Versus High-Fidelity Models

Simplified models use constant atmosphere, point-mass dynamics, and idealized guidance. They are suitable for sensitivity analysis and for understanding trends. High-fidelity models would require validated aerodynamic data, structural mass breakdowns, propulsion curves, and control systems. This project does not currently claim high fidelity.

### 7.5 Real Reusable Vehicles as References

Publicly documented reusable launch vehicles illustrate the engineering problems this simulation addresses, including recovery and reuse. The project uses public information only. It does not reproduce any company's proprietary systems, flight software, control algorithms, or performance data, and makes no claim of equivalence to any operational vehicle.

## 8. System Models and Governing Equations

The project is organized into models that exchange defined quantities. Each model has its own equations, which must be documented alongside its code and assumptions.

| Model | Key equations or relations | Principal outputs |
|---|---|---|
| Data-centre thermal | $\dot{Q} = \dot{m} c_p \Delta T$; exchanger effectiveness | Heat load, flow, temperatures |
| Heat recovery | Exergy or temperature-limited heat transfer | Usable thermal energy and its temperature |
| Water processing | Mass balance; energy per unit product water | Product water, brine, energy demand |
| Propellant production | Stoichiometry; electrical energy per kg H₂; liquefaction energy | Hydrogen and oxygen mass, energy demand |
| Rocket dynamics | Newton's second law; ideal rocket equation; drag | Trajectory, $\Delta v$, propellant use, landing state |
| Integrated analysis | Material and energy balances across modules | Feasibility, external energy requirement, bottlenecks |

## 9. Data Flow and Module Interfaces

### 9.1 Module Chain

The intended flow of information is:

1. The data-centre thermal model outputs heat load, coolant temperatures, and flow requirements.
2. The heat-recovery model estimates usable thermal energy at each temperature and the operating constraints that apply.
3. The water-processing model estimates product water quantity and the treatment energy required.
4. The propellant model estimates hydrogen and oxygen production rates, electrical demand, and available propellant mass.
5. The rocket model takes vehicle and propellant parameters and simulates flight and recovery.
6. The integrated analysis compares resource availability with energy demand and reports feasibility.

### 9.2 Input and Output Classification

Every parameter should be labeled as one of:

- **Measured:** obtained from site or hardware measurement, with source and uncertainty recorded.
- **Literature-derived:** taken from a published reference, with citation.
- **Calculated:** produced by a model from other parameters.
- **Assumed:** chosen for the analysis, with rationale and sensitivity range.
- **Unknown:** not yet determined. Shown as `[TBD]`.

### 9.3 Interface Table

| Interface | Quantity | Units | Classification | Value |
|---|---|---|---|---|
| Thermal → Heat recovery | Heat rejection rate | W | Calculated | `[TBD]` |
| Thermal → Heat recovery | Seawater outlet temperature | °C | Calculated | `[TBD]` |
| Water processing → Propellant | Purified water supply | kg/s | Calculated | `[TBD]` |
| External → Propellant | Electrical power | W | Assumed | `[TBD]` |
| Propellant → Rocket | Hydrogen and oxygen mass | kg | Calculated | `[TBD]` |
| Rocket → Integrated analysis | Required $\Delta v$ and reserve | m/s, kg | Calculated | `[TBD]` |

## 10. Installation and Usage

Commands, dependencies, and file layout depend on the implementation and are not documented here.

**Language and runtime:** `[TBD]`

**Dependencies:** `[TBD: numerical, thermodynamic, and property libraries]`

**Setup:** `[TBD: installation steps from repository]`

**Running a model:** `[TBD: entry points and configuration files]`

**Input format:** `[TBD: parameter file schema, including units]`

Users should check that every input carries units and classification as described in [Section 9.2](#92-input-and-output-classification).

## 11. Example Simulations and Visualizations

No example outputs are reported in this document. Planned examples include:

- Seawater temperature rise and pumping power across a range of heat loads `[TBD]`.
- Usable heat as a function of temperature level `[TBD]`.
- Electrical energy and mass production for a stated electrolysis scenario `[TBD]`.
- Ascent and descent trajectories for a stated vehicle configuration `[TBD]`.
- Sensitivity of $\Delta v$ and landing propellant reserve to mass and drag `[TBD]`.

Figures will be added with their input files and the commit or version that produced them.

## 12. Validation and Quantitative Analysis

### 12.1 Validation Criteria

- Thermal models agree with hand heat-balance calculations for simple cases.
- Heat-exchanger predictions are compared with published correlations or reference data `[TBD: reference case]`.
- Water and mass balances close to within a stated tolerance.
- Propellant production estimates are consistent with stoichiometry and with the stated energy demand.
- Trajectory integration converges as step size decreases.
- Simplified trajectory outputs are compared against analytical solutions, such as the ideal rocket equation, and against public reference cases.
- Integrated energy accounting identifies all external energy inputs.

### 12.2 Benchmark Table

| Check | Reference | Model result | Difference | Status |
|---|---|---|---|---|
| Heat balance, simple exchanger | Hand calculation | `[TBD]` | `[TBD]` | Not yet performed |
| Mass balance closure, water treatment | Stated input mass | `[TBD]` | `[TBD]` | Not yet performed |
| Electrolysis energy per kg H₂ | Cited reference | `[TBD]` | `[TBD]` | Not yet performed |
| Ideal rocket equation check | Analytical value | `[TBD]` | `[TBD]` | Not yet performed |
| Trajectory convergence | Smaller step size | `[TBD]` | `[TBD]` | Not yet performed |

### 12.3 Levels of Confidence

Four levels should be kept separate in all reporting:

- **Conceptual feasibility:** whether the physics permits the proposed connection.
- **Mathematical simulation:** whether the model is internally consistent and numerically converged.
- **Experimental validation:** whether model predictions match measured data.
- **Operational deployment:** whether the system has been built and run.

This project currently addresses the first two. It has not reached experimental validation or deployment.

## 13. Engineering Assumptions and Current Limitations

- Heat-exchanger, pumping, and water-treatment models use assumed or site-dependent parameters that have not been measured.
- Electrolysis energy figures are literature-based and technology-dependent.
- Liquefaction and storage energy depend strongly on scale and process design, and are not yet estimated.
- The rocket model is simplified and does not capture structural, thermal, or control detail.
- Waste heat is assumed to be at low temperature, and its use is limited accordingly.
- Integrated feasibility is not established. Energy import requirements are expected to be large relative to the thermal resource.
- No production capacity, cost, or energy-independence figure is claimed.

## 14. Environmental, Safety, and Feasibility Considerations

### 14.1 Environmental

- Seawater intake and discharge affect marine ecosystems. Thermal discharge, chemical treatment residues, and brine must be assessed against local permitting requirements.
- Desalination brine has high salinity and may contain treatment chemicals.
- Electricity sourcing determines the lifecycle emissions of hydrogen and oxygen production, and must be reported explicitly.

### 14.2 Safety

- Hydrogen and oxygen handling requires gas detection, ventilation, segregation, and pressure protection.
- Cryogenic systems present burn, embrittlement, and oxygen-enrichment hazards.
- Rocket operations are subject to regulation and are outside the scope of this project's current work.

### 14.3 Feasibility

The integrated system is feasible only if the external energy supply is adequate and the process chain closes. The waste-heat resource is unlikely to be the decisive factor for electrolysis, liquefaction, or propulsion. Feasibility will be judged on energy balance first, then on cost, siting, and regulation, none of which are assessed here.

## 15. Development Roadmap

Planned milestones. None is complete unless confirmed.

**Phase 1 — Physical foundations.** Establish assumptions, parameters, and standalone thermodynamic and trajectory models.

**Phase 2 — Cooling simulation.** Model data-centre heat loads, seawater heat exchange, pumping demand, and environmental constraints.

**Phase 3 — Water and resource processing.** Model desalination or purification where needed and quantify water availability and treatment energy.

**Phase 4 — Propellant pathway.** Implement a mass and energy balance for a selected production route, including electrical inputs and process losses.

**Phase 5 — Reusable vehicle dynamics.** Develop and validate ascent, flight, descent, and recovery simulations at an appropriate fidelity.

**Phase 6 — System integration.** Connect module outputs and inputs, perform resource and energy accounting, and identify infeasible assumptions or bottlenecks.

**Phase 7 — Optimization and visualization.** Explore parameter sensitivity, scenario comparisons, resource allocation, and performance trade-offs.

## 16. References and Technical Resources

References will be added with full citations as they are used. Candidate categories:

- Heat-transfer and heat-exchanger texts for seawater cooling correlations `[TBD]`.
- Electrolysis and hydrogen-production technology reviews, including energy-consumption data `[TBD]`.
- Hydrogen and oxygen liquefaction process literature `[TBD]`.
- Rocket propulsion and orbital mechanics textbooks, including the ideal rocket equation `[TBD]`.
- Public engineering documentation for reusable launch vehicles, used as reference only `[TBD]`.

## 17. Third-Party Acknowledgements

Third-party software, data sources, and published models used in the project will be credited here once confirmed `[TBD]`. Trademarks and product names referenced in this document belong to their respective owners and are used descriptively. The project is not affiliated with, sponsored by, or endorsed by any company whose vehicles or facilities are mentioned.

## 18. Contribution Guidelines

Contributions are welcome in thermal modeling, process engineering, electrochemistry, trajectory simulation, numerical methods, and documentation. Contributors should:

- State units and classify every parameter as measured, literature-derived, calculated, assumed, or unknown.
- Provide a validation case for any new model.
- Report residuals for conservation checks.
- Avoid presenting simplified results as flight or operational estimates.

Detailed contribution procedures `[TBD: to be added by the maintainer]`.

## 19. License

`[TBD: license to be confirmed by the project owner. No license is asserted in this document.]`
