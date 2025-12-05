# Architecture Overview

We propose a clean separation between:

- **Bonsai operators for acquisition** (provided by `ucl-open/acquisition`)
- **Python schemas describing experiments** (provided by `open_ucl_rigs`)

Together, they make experiments **config-driven**, **fully logged**, and **schema-described**, so data can always be read and interpreted correctly later.

---

## 1. Bonsai: `ucl-open/acquisition` (Name TBD)

**NuGet package:** `UclOpen.Acquisition` 

Provides standardised Bonsai **operators** for:

- Hardware (`UclOpen.Acquisition.Device`)
- Logging (`UclOpen.Acquisition.Logging`)
- Shared utilities (`UclOpen.Acquisition.Utilities`)

These operators read a YAML config and configure devices + logging in a consistent way across rigs and labs.

---

## 2. Python: `ucl_open_rigs` (Name TBD)

Provides **core Pydantic schemas** that describe all parts of an experiment:

- **Rig** (`rig.py`)
- **Task** (`task.py`)
- **Experiment** (`experiment.py`)

Experimenters / designers extend these schemas for their own experiments on a new repo.

This can then be run to produce:

- `experiment.json` — a combined schema detailing all experiment parameters, including rig configuration and metadata.

---
## 3. Dotnet: `Bonsai.sgen` 

Automatically generates corresponding C# classes and associated operators in `Extensions` from the `.json` schema. These are then available in the Bonsai editor Toolbox, and can be wired in directly with experimenter defined friendly names and structure. 

## 4. During an experiment

1. Bonsai loads `experiment.yml`.
2. `UclOpen.Acquisition` operators configure hardware + logging using these parameters.
3. Data is recorded along with the configuration parameters + schema.

---

## 5. Reading the data (`ucl_open_api`) (Name TBD)

Python readers use the schema to interpret:

- which devices and data streams exist
- how they map to each rig
- which workflow + git commit produced the data (To allow complete recreation of experiment)

---
