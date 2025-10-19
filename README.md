# 📁 Folder Structure

### 🔹 Source Code
The **`source_code`** directory contains two main subcomponents:

1. **Python Simulator (`Py-Simulator/`)**  
   Implements the full digital-twin production line with reinforcement learning and rule-based scheduling.  
   It simulates dynamic device status, queue balancing, and fault recovery mechanisms.

2. **SystemJ Exemplar Factory (`SystemJ-Factory/`)**  
   This component serves as a **prototype of a distributed control system** modeled in **SystemJ**, a synchronous reactive language designed for real-time embedded and cyber-physical systems.
   #### 🎯 Purpose
    The exemplar factory demonstrates how a *reactive control layer* could manage physical or simulated devices (e.g., fillers, cappers, labelers) using synchronous signals and deterministic scheduling. It provides a conceptual and experimental base for future integration with the Python-based digital twin.
   #### 🧩 Current Implementation
    At the current stage, the SystemJ factory:
    - Includes several **control components (CDs)** representing factory workstations and communication channels.
    - Demonstrates **factory workflow progression**, such as bottle movement, workstation activation, and timing signals.
    - Can be executed via:
     ```bash
        ./gradlew run

### 🔹 Project Files
The **`project_files`** folder contains design records, progress updates, and communication materials exchanged with the supervisor (Dr. Zoran Salcic).
These files document project evolution and architecture decisions.

### 🔹 Results
The **`results`** folder stores simulation outputs and evaluation data from the Python simulator.
These include performance comparisons (RL vs Rule-based) data and visualized graphs of completion time, utilization, and load balance.

# Py-Simulator — Digital Twin–Driven Factory Simulation

This folder contains the Python-based simulation framework that models and evaluates a **digital-twin production line** for intelligent scheduling, fault recovery, and reinforcement learning (RL).  

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

### **Digital Twin Production Model**
Each physical factory component (fillers, cappers, labelers, conveyors) is modeled as a digital twin with:
- Realistic operational timing (`filler_time`, `cap_time`, `label_time`)
- Fault and recovery mechanisms
- Queue-based workpiece (bottle) flow
- Status tracking (`Working`, `Idle`, `Fault`, `Unavailable`)

These are implemented as `Device`, `Conveyor`, `BufferNode`, and `Bottle` classes.

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
- `normal_test.py` automate experiments by:
- Scaling production orders (10–10,000 bottles)
- Comparing RL vs non-RL (rule-based) performance

- `fault_test.py` automate experiments by:
- Injecting random filler faults (up to 50% per type)
- Comparing RL vs non-RL (rule-based) performance

#### Metrics:
| Metric | Description |
|---------|-------------|
| `actual_completion_time` | Total time to complete all bottles |
| `avg_time` | Mean production time per bottle |
| `avg_utilization` | Average utilization rate of filler devices |
| `balance_degree` | Load balance across all fillers |

Results are visualized and stored in temporary directories created during execution.

---

## 🚀 Quick Start

### 1. **Install Dependencies**
   ```bash
      pip install numpy matplotlib pandas
   ```

### 2. **Run Simulation**
   ```bash
      python normal_test.py
      python fault_test.py
   ```
