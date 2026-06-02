# Bus Fleet Scheduling and Energy Management Tool

## Overview

The Bus Fleet Scheduling and Energy Management Tool is a Python-based transport planning application for generating operational bus schedules while evaluating fleet energy requirements, refuelling or recharging events, and vehicle utilisation.

The application produces vehicle schedules between two terminals based on route characteristics and service frequency requirements. Beyond traditional timetable outputs, the system assesses vehicle range limitations, predicts service or charging requirements, and communicates operational impacts through schedule tables, downtime summaries, and space-time diagrams.

The tool supports diesel, battery-electric (BEV), hydrogen fuel-cell (FCEV), hybrid, and compressed natural gas (CNG) buses, allowing transport planners to compare operational implications across different fleet technologies side by side.

---

## Core Capabilities

### Route and Service Configuration

Users define:

- Origin and destination terminals
- One-way route distance (km)
- One-way travel time (minutes)
- Departure headway (minutes between services)
- Operating period (start and end times)
- Departure date

A default terminal layover time of 15% of travel time is calculated automatically and can be overridden to reflect local operating conditions.

---

### Multi-Technology Fleet Modelling

The tool ships with five configurable vehicle profiles:

| Bus Type | Energy Source | Default Range | Default Service Time |
|---|---|---|---|
| Diesel | Diesel | 600 km | 10 min |
| Battery-Electric (BEV) | Electric | 250 km | 45 min |
| Hydrogen Fuel-Cell (FCEV) | Hydrogen | 400 km | 8 min |
| Hybrid (Diesel-Electric) | Hybrid | 450 km | 12 min |
| CNG | Compressed Natural Gas | 350 km | 10 min |

All parameters can be overridden at runtime to model specific fleet configurations or procurement scenarios.

---

### Automatic Fleet Sizing

The software determines:

- Total round-trip cycle time
- Minimum fleet size required for the given headway
- Adjusted operational cycle time
- Schedule efficiency (%)

Fleet size is derived from the ratio of cycle time to headway, ensuring adequate vehicle coverage across the entire operating period.

---

### Vehicle Assignment and Trip Scheduling

Vehicles are assigned cyclically across departures to balance fleet utilisation. For each trip the system calculates:

- Departure from Terminal A
- Arrival at Terminal B
- Departure from Terminal B (after layover)
- Return arrival at Terminal A

---

### Energy Consumption and Service Event Modelling

The tool tracks accumulated vehicle mileage continuously and triggers a refuelling or recharging event when a vehicle reaches **90% of its usable operating range**.

For each service event the system records:

- Vehicle identifier
- Service start and end time
- Distance travelled at the trigger point
- Whether the event fits within the scheduled layover window
- Additional operational delay introduced, if any

This enables planners to evaluate the practical feasibility of alternative propulsion technologies under real-world service patterns.

---

### Operational Downtime Assessment

The system checks whether refuelling or charging can be completed within normal terminal layovers. Where service durations exceed available layover time, the tool identifies:

- Potential service disruptions
- Additional delay requirements
- Vehicle-level operational constraints

This is especially relevant for battery-electric and hydrogen fleets, where energy replenishment strategy has a direct bearing on scheduling feasibility.

---

## Outputs

### Schedule Table

A colour-coded timetable showing vehicle assignment, departure times, arrival times, and terminal turnaround periods. Each vehicle is assigned a distinct colour for quick identification.

### Space-Time Diagram

A graphical representation of all vehicle movements, showing:

- **Solid diagonal lines** — outbound movements (A → B)
- **Dashed diagonal lines** — return movements (B → A)
- **Dotted horizontal segments** — terminal layovers at B
- **Red shaded bands** — service or charging downtime events

Downtime events are annotated directly on the diagram, enabling visual identification of scheduling bottlenecks.

### Downtime Summary Table

A dedicated report listing every service event with:

- Vehicle affected
- Service start and end times
- Odometer reading at trigger
- Layover compliance (fits / does not fit)
- Extra delay incurred

### CSV Export

All outputs are written to a single CSV file containing three sections:

1. **Trip Schedule** — vehicle assignments and departure/arrival times
2. **Fleet Configuration** — technology, energy type, range, service duration
3. **Service Events** — refuelling/charging timing, trigger distances, delay impacts

The export is compatible with Excel, Power BI, and third-party transport planning tools.

---

## How to Run

```bash
python bus_scheduler.py
```

The program runs interactively and prompts for all required inputs in sequence:

1. Origin and destination terminal names
2. One-way distance (km) and travel time (minutes)
3. Terminal layover time (default or manual override)
4. Departure headway (minutes)
5. Departure date and operating period
6. Bus type selection and optional parameter overrides

No configuration files or external databases are required.

---

## Dependencies

| Package | Purpose |
|---|---|
| `matplotlib` | Schedule table, space-time diagram, downtime summary |
| `csv` | CSV export (standard library) |
| `math`, `colorsys`, `collections`, `datetime` | Core calculations (standard library) |

Install with:

```bash
pip install matplotlib
```

---

## Future Development Opportunities

Potential enhancements identified for future versions:

1. State-of-charge modelling for battery-electric fleets
2. Depot-based charging and refuelling scheduling
3. Real-time traffic and travel-time integration
4. Passenger demand modelling
5. Named vehicle registration and fleet assignment
6. Multi-route network scheduling
7. Graphical User Interface (GUI)
8. Database integration for persistent schedule storage
9. Cost, emissions, and energy consumption analysis
10. Integration with transport management systems and AVL platforms
