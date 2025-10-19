# 📁 Folder Structure

### 🔹 Source Code
The **`source_code`** directory contains two main subcomponents:

1. **Python Simulator (`Py-Simulator/`)**  
   Implements the full digital-twin production line with reinforcement learning and rule-based scheduling.  
   It simulates dynamic device status, queue balancing, and fault recovery mechanisms.

2. **SystemJ Exemplar Factory (`SystemJ-Factory/`)**  
   A synchronous control system model built using **SystemJ**.  
   It can be executed via:
   ```bash
   ./gradlew run


# Py-Simulator — Digital Twin–Driven Factory Simulation

This folder contains the Python-based simulation framework that models and evaluates a **digital-twin production line** for intelligent scheduling, fault recovery, and reinforcement learning (RL)–based optimization.  
It is part of the project **“Digital Twins and Machine Learning in Industrial Automation Systems Control.”**

---

## 📂 Folder Overview

| File | Description |
|------|--------------|
| **RL_main.py** | Implements the *Reinforcement Learning–enhanced production line*, including environment state representation, DQN agent, and action space definitions. |
| **Rule_main.py** | Implements the *rule-based baseline system* for production scheduling and device fault handling without RL. |
| **normal_test.py** | Runs scalability comparison between RL and rule-based systems under normal (non-faulty) conditions. |
| **fault_test.py** | Runs robustness and fault-injection experiments, simulating random device failures and evaluating RL adaptability. |

---

## ⚙️ Core Design

### 1. **Digital Twin Production Model**
Each physical factory component (fillers, cappers, labelers, conveyors) is modeled as a digital twin with:
- Realistic operational timing (`filler_time`, `cap_time`, `label_time`)
- Fault and recovery mechanisms
- Queue-based workpiece (bottle) flow
- Status tracking (`Working`, `Idle`, `Fault`, `Unavailable`)

These are implemented in `Rule_main.py` as `Device`, `Conveyor`, `BufferNode`, and `Bottle` classes.

---

### 2. **Reinforcement Learning Controller**
Defined in `RL_main.py`, the RL controller observes the factory state and decides:
- **Release interval** — how fast new bottles are introduced.
- **Assignment strategy** — whether to balance load or select the shortest queue.
- **Fault response** — pause, emergency reroute, or conservative processing.
- **Recovery priority** — which fault to repair first.

#### Main Components:
- `RLState`: Converts the entire production line into a high-dimensional numerical state vector (including queue lengths, fault ratios, utilization, etc.).
- `RLActionSpace`: Defines all discrete action combinations.
- `DQNAgent`: Implements a fault-aware deep Q-learning agent with replay buffers and adaptive exploration.
- `ProductionLineRL`: Connects the agent with the simulated environment.

---

### 3. **Simulation and Testing Framework**
Both `normal_test.py` and `fault_test.py` automate experiments by:
- Scaling production orders (10–10,000 bottles)
- Injecting random filler faults (up to 50% per type)
- Comparing RL vs non-RL (rule-based) performance

#### Metrics:
| Metric | Description |
|---------|-------------|
| `actual_completion_time` | Total time to complete all bottles |
| `avg_time` | Mean time per bottle |
| `avg_utilization` | Average utilization rate of filler devices |
| `balance_degree` | Load balance across all fillers |
| `working_fillers` | Active fillers after fault injection |

Results are visualized and stored in temporary directories created during execution.

---

## 🚀 Quick Start

### 1. **Install Dependencies**
```bash
pip install numpy matplotlib pandas
