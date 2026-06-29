CASPER: Volume 3 — Executive Brain Specification

Version 1.0.0-EXEC

Classification: Restricted — Executive Systems Engineering Division

Section 1: Executive Philosophy

1.1 Architectural Rationale

An autonomous agent operating within an open-world, real-time environment cannot
rely on direct execution loops. Large Language Model (LLM) inference is
fundamentally a stateless, token-by-token sequence generation process. It lacks
self-directed temporal awareness, resource sensitivity, and native
interrupt-handling capabilities.

Traditional operating system (OS) kernels execute deterministic threads on
deterministic silicon, scheduling processes based on predictable CPU bounds and
I/O cycles.

Project CASPER implements a dedicated cognitive scheduling layer: the Executive
Brain.

+-------------------------------------------------------------------------+
|                         CASPER EXECUTIVE BRAIN                          |
|                                                                         |
|  +--------------------+   +---------------------+   +----------------+  |
|  |  LLM Inference     |   | OS Kernel Schedulers|   | Agent Loops    |  |
|  |  - Stateless       |   | - Deterministic     |   | - Closed DAGs  |  |
|  |  - Token-bound     |   | - CPU / IO cycles   |   | - No preempt   |  |
|  +--------------------+   +---------------------+   +----------------+  |
|            \                         |                         /        |
|             \------------------------+------------------------/         |
|                                      |                                  |
|                                      v                                  |
|                 Real-Time Cognitive Resource Allocation                 |
|                 Asynchronous Task-Set Orchestration                    |
|                 Deterministic Guardrail Enforcement                    |
+-------------------------------------------------------------------------+

The Executive Brain sits between non-deterministic AI generation models and
deterministic host computers, managing the flow of active thoughts, goals, and
execution tasks.

1.2 Comparison of Execution Schedulers

| Attribute               | LLM Inference                                | Traditional OS Kernel             | Agent Frameworks (e.g., AutoGPT)    | Human Executive Cortex         | CASPER Executive Brain                          |
| :---------------------- | :------------------------------------------- | :-------------------------------- | :---------------------------------- | :----------------------------- | :---------------------------------------------- |
| **Execution Model**     | Stateless token prediction                   | Deterministic thread scheduling   | Sequential loop execution           | Neural synchronization, gating | Asynchronous, event-driven cognitive scheduling |
| **Interrupt Latency**   | Bound to sequence generation ($O(N)$ tokens) | Sub-microsecond (Hardware-bound)  | High latency; polling-based         | Variable (100ms - 300ms)       | Low latency ($\leq 10$ms via preemptive loops)  |
| **Resource Allocation** | Static context windows                       | Dynamic CPU/Memory bounds         | No resource throttling              | Metabolic resource throttling  | Multi-tiered budgets (Tokens, CPU, Cost, Time)  |
| **Goal Persistence**    | Lost across sessions                         | Static process trees              | Ephemeral script targets            | Highly robust, associative     | Persistent hierarchical Goal Lattice            |
| **State Recovery**      | None (Requires external context loading)     | Core dump, transactional database | Manual restart of execution loops   | Associative memory reload      | Task State Block (TSB) restoration              |
| **Safety Enforcement**  | System instructions                          | POSIX permissions, sandbox layers | None (Relies on system permissions) | Inhibitory neural pathways     | Hardcoded policy-guard gates                    |

1.3 The Probabilistic Execution Problem

The primary problem in AI orchestration is the translation of probabilistic
thoughts into deterministic actions. When an AI planning system proposes a plan
step, the system cannot guarantee that the action will execute within expected
limits, terminate cleanly, or respect safety boundaries.

The Executive Brain solves this by treating cognitive actions as preemptible
tasks scheduled within a secure execution sandbox. It monitors execution
progress, tracks resources, and manages interrupts, ensuring the system remains
stable and responsive even when processing complex, non-deterministic tasks.

Section 2: Executive Brain Architecture

                                    +-----------------------+
                                    |       USER/HOST       |
                                    +-----------------------+
                                        |             ^
                      User Intent / I/O |             | Context HUD / Action
                                        v             |
+-----------------------------------------------------------------------------------------+
|                                    CONTROL PLANE                                        |
|                                                                                         |
|  +--------------------------------+           +--------------------------------------+  |
|  |      Attention Controller      |---------->|           State Controller           |  |
|  |  (Gating, focus, lock targets) |           | (System transitions: Think, Execute) |  |
|  +--------------------------------+           +--------------------------------------+  |
|                 |                                                 |                     |
|                 v                                                 v                     |
|  +--------------------------------+           +--------------------------------------+  |
|  |       Interrupt Manager        |           |            Policy Engine             |  |
|  |   (IVT, nested loop signals)   |           |    (Execution constraints, safety)   |  |
|  +--------------------------------+           +--------------------------------------+  |
+-----------------------------------------------------------------------------------------+
        |                                                                           ^
        | Event Dispatch                                                            | State
        v                                                                           | Sync
+-----------------------------------------------------------------------------------------+
|                                  SCHEDULING PLANE                                       |
|                                                                                         |
|  +-----------------------------------------------------------------------------------+  |
|  |                                  Task Scheduler                                   |  |
|  |     (Multi-level feedback queues: Emergency, Active, Background, Delayed)         |  |
|  +-----------------------------------------------------------------------------------+  |
|                 |                                                 ^                     |
|                 | Task Dispatch                                   | Metric Feedback     |
|                 v                                                 |                     |
|  +-----------------------------------------------------------------------------------+  |
|  |                                 Resource Manager                                  |  |
|  |              (Token metrics, memory usage, API costs, compute limits)             |  |
|  +-----------------------------------------------------------------------------------+  |
+-----------------------------------------------------------------------------------------+
        |                                                                           ^
        | Orchestration Signals                                                     | Run Logs
        v                                                                           |
+-----------------------------------------------------------------------------------------+
|                                   EXECUTION PLANE                                       |
|                                                                                         |
|  +-----------------------------------------------------------------------------------+  |
|  |                                 Execution Manager                                 |  |
|  |          (Task execution slots, concurrent workers, transactional rollback)       |  |
|  +-----------------------------------------------------------------------------------+  |
|                 |                                                 ^                     |
|                 | Task Commands                                   | Result Streams      |
|                 v                                                 |                     |
|  +-----------------------------------------------------------------------------------+  |
|  |                               Skills & Actuators                                  |  |
|  |                    (Browsers, terminal shells, local file systems)                |  |
|  +-----------------------------------------------------------------------------------+  |
+-----------------------------------------------------------------------------------------+

2.1 Control Plane

  - Role: Manages the system's operational focus, safety rules, and high-level
    states.
  - Key Subsystems: Attention Controller, Interrupt Manager, State Controller,
    Policy Engine.
  - Data Flow: Receives raw inputs and events from the host operating system,
    filters them for significance, and issues execution commands to the
    Scheduling Plane.

2.2 Scheduling Plane

  - Role: Organizes task execution schedules, balances workloads, and allocates
    system resources.
  - Key Subsystems: Task Scheduler, Resource Manager.
  - Data Flow: Translates selected tasks into managed queue entries, monitoring
    execution times and resource consumption to prevent bottlenecks.

2.3 Execution Plane

  - Role: Runs individual task steps and coordinates active system tools.
  - Key Subsystems: Execution Manager, Skills & Actuators.
  - Data Flow: Dispatches tasks to sandboxed workers, monitors process
    performance, and returns execution feedback to the Control Plane.

Section 3: Executive Kernel

                                  [EXECUTIVE KERNEL]
                                          |
          +-------------------------------+-------------------------------+
          |                               |                               |
          v                               v                               v
 [Asynchronous Dispatcher]     [State Controller Core]        [Event Routing Engine]
  - Dispatches task signals     - Tracks dynamic system states  - Manages internal alerts
  - Manages IPC/gRPC channels   - Controls mode transitions     - Routes events to queues

The Executive Kernel serves as the core coordinator of CASPER, managing task
states, system events, and resource allocations across the entire cognitive
architecture.

3.1 Kernel Subsystems Specification

| Subsystem             | Responsibilities                                                                     | Core Interfaces                          | Inputs                                   | Outputs                            | Performance Targets          | Failure Modes                    | Recovery Protocol                                                            |
| :-------------------- | :----------------------------------------------------------------------------------- | :--------------------------------------- | :--------------------------------------- | :--------------------------------- | :--------------------------- | :------------------------------- | :--------------------------------------------------------------------------- |
| **Executive Core**    | Coordinates boot-up, manages system shutdown, and supervises active subsystems.      | `init()`, `shutdown()`, `status()`       | Boot flags, system shutdown signals      | Active subsystem instances         | Boot sequence under 100ms    | Subsystem initialization timeout | Reinitialize target system, fall back to safe configuration, or issue alert. |
| **Dispatcher**        | Allocates execution signals to queues and handles communication between subsystems.  | `dispatch()`, `route_task()`             | Execution requests, action profiles      | Assigned queue identifiers         | Message routing under 1ms    | Channel capacity overload        | Scale up queue workers, block incoming tasks, and apply backoff.             |
| **Scheduler**         | Manages multi-tier queues, schedules tasks, and balances operational priorities.     | `schedule()`, `reorder_queue()`          | Dynamic task entries, priority values    | Ordered task execution sequences   | Queue sorting under 2ms      | Starvation / Deadlock state      | Force priority promotions and run cycle-detection audits.                    |
| **Interrupt Manager** | Monitors and routes interrupts, managing priorities and nested interrupts.           | `raise_interrupt()`, `mask_vector()`     | Event alerts, hardware triggers          | Preemption commands, focus alerts  | Preemption under 5ms         | Interrupt loop / Stack overflow  | Reset interrupt stack, temporarily mask events, resume from last safe step.  |
| **Resource Manager**  | Tracks system resource usage, balancing token, budget, and memory limits.            | `allocate()`, `revoke()`, `query()`      | Metric streams, limits configuration     | Allocated resource tokens          | Query latency under 1ms      | Resource allocation overrun      | Halt lower-priority tasks, reclaim resources, notify execution manager.      |
| **Context Manager**   | Stores and restores active task context schemas and focus parameters.                | `freeze()`, `hydrate()`, `save()`        | Active task records, variables           | Hydrated task state blocks (TSBs)  | Context switch under 10ms    | Context block corruption         | Rebuild context from parent logs, fall back to last saved state checkpoint.  |
| **Execution Manager** | Launches task workers, allocates execution slots, and manages transaction rollbacks. | `execute()`, `rollback()`, `terminate()` | Target task steps, environment variables | Execution states, result logs      | Process startup under 5ms    | Worker crash / Timeout error     | Terminate hung processes, trigger rollback routines, reschedule task step.   |
| **State Controller**  | Tracks dynamic system-wide states and schedules mode transitions.                    | `get_state()`, `transition()`            | Transition commands, task updates        | Active state identifiers           | Transition latency under 1ms | Invalid state transition attempt | Block transition, rollback to last stable state, log exception.              |
| **Event Controller**  | Manages internal system alerts and directs events to active queue filters.           | `publish()`, `subscribe()`               | Process event logs, telemetry streams    | Event update notifications         | Event delivery under 2ms     | Event queue saturation           | Prune redundant events, drop low-priority logs, scale event buffer.          |
| **Policy Engine**     | Checks active tasks against system security and privacy rules.                       | `verify()`, `audit()`                    | Proposed actions, data payloads          | Verification verdicts (Allow/Deny) | Verification under 5ms       | Safety validation timeout        | Block target action, isolate task worker, request manual user confirmation.  |

Section 4: Attention Controller

4.1 Attention Allocation Logic

The Attention Controller acts as CASPER's cognitive gating system, allocating
processing capacity based on goal relevance and event urgency:

\mathcal{A}(t) = \int_{0}^{t} \left[ w_1 \cdot \mathcal{R}(g_i, c(x)) + w_2 \cdot \mathcal{U}(e_j) - w_3 \cdot \mathcal{D}(t) \right] dt

Where:

  - \mathcal{R}(g_i, c(x)) represents the semantic relevance of active goal g_i
    within context c(x).
  - \mathcal{U}(e_j) is the urgency score of incoming environmental event e_j.
  - \mathcal{D}(t) represents the distraction decay coefficient over time.
  - w_1, w_2, w_3 are prioritization weights adjusted dynamically by the system.

flowchart TD
    A[Incoming Percept / Event] --> B[Calculate Relevance score & Urgency score]
    B --> C[Compute Salience metric]
    C --> D{Salience > Active Attention Threshold?}
    
    D -->|Yes| E{Is Attention Lock Active?}
    D -->|No| F[Queue Event to Background Queue]
    
    E -->|Yes| G{Interrupt Level > Attention Lock Level?}
    E -->|No| H[Trigger Preemption Context Switch]
    
    G -->|Yes| H
    G -->|No| I[Queue Alert to Delayed Queue]

4.2 Attention Lock Mechanics

To maintain performance during complex, high-precision tasks, the Attention
Controller employs a system of Attention Locks:

stateDiagram-v2
    [*] --> Unlocked : Default Idle State
    
    Unlocked --> Locked : Set Attention Lock (Level N)
    state Locked {
        [*] --> FocusPreservation
        FocusPreservation --> EvaluateInterrupt : Event Alert
        EvaluateInterrupt --> FocusPreservation : Interrupt Priority <= N (Masked)
    }
    
    Locked --> Unlocked : Release Lock / Task Completed
    Locked --> ContextPreempted : Interrupt Priority > N (Preemption Triggered)
    
    ContextPreempted --> Locked : Restore Context State

4.3 Focus Horizon Dynamics

The Attention Controller dynamically manages the Focus Horizon, restricting the
volume of information loaded into active reasoning contexts to prevent cognitive
overload:

+-------------------------------------------------------------------------+
|                         ATTENTION FOCUS WINDOW                          |
+-------------------------------------------------------------------------+
|                                                                         |
|  [ IMMEDIATE FOCUS WINDOW ]                                             |
|  Current Action Path (Active TSB)                                       |
|  Allocated processing: 80%                                              |
|                                                                         |
|  [ CONTEXT WINDOW HORIZON ]                                             |
|  Active Goal Lattice & Memory Associations                             |
|  Allocated processing: 15%                                              |
|                                                                         |
|  [ PERIPHERAL HORIZON ]                                                 |
|  System Notifications & Delayed Event Logs                              |
|  Allocated processing: 5%                                               |
|                                                                         |
+-------------------------------------------------------------------------+

Section 5: Task Scheduler

                                 [TASK SCHEDULER]
                                        |
         +------------------------------+------------------------------+
         |                              |                              |
         v                              v                              v
[Emergency Priority Queue]     [Active Execution Queue]       [Delayed Tasks Queue]
 - Non-preemptible tasks        - Active planning tasks        - Scheduled events
 - Preemption latency < 1ms     - Round-Robin scheduler        - Low-priority background runs

5.1 Multi-Tier Queues

The Task Scheduler organizes execution tasks across six dedicated, dynamically
prioritized queues to balance immediate responsiveness with long-term background
processing:

flowchart TD
    In[Inbound Task Request] --> Type{Analyze Task Priority}
    
    Type -->|Level 0: Emergency| EQ[Emergency Queue: Preemptive Execution]
    Type -->|Level 1: Active| AQ[Active Queue: Multi-Step Tasks]
    Type -->|Level 2: Background| BQ[Background Queue: System Consolidation]
    Type -->|Level 3: Waiting| WQ[Waiting Queue: Blocked on Dependencies]
    Type -->|Level 4: Delayed| DQ[Delayed Queue: Scheduled Tasks]
    Type -->|Level 5: Dependency| DepQ[Dependency Queue: Context Checks]
    
    EQ --> Dis[Task Dispatcher Core]
    AQ --> Dis
    BQ --> Dis

5.2 Dynamic Queue Metrics and Throttling

| Queue Identifier     | Base Priority        | Preemption Rule             | Allocation Target                     | Max Concurrent Capacity | Starvation Prevention Rule                                          |
| :------------------- | :------------------- | :-------------------------- | :------------------------------------ | :---------------------- | :------------------------------------------------------------------ |
| **Emergency Queue**  | Priority 0 (Highest) | Non-preemptible             | Immediate hardware runtime allocation | 1 active execution slot | Bypass scheduling; executes immediately.                            |
| **Active Queue**     | Priority 1           | Preemptible by Priority 0   | 60% of available CPU cycles           | 4 concurrent slots      | Dynamic priority promotion after 5 seconds of delay.                |
| **Background Queue** | Priority 2           | Preemptible by Priority 0/1 | 20% of available CPU cycles           | 8 concurrent slots      | Promotion to Active Queue after 30 seconds of starvation.           |
| **Waiting Queue**    | Priority 3           | Non-executable (Blocked)    | 0% (Holds tasks blocked on inputs)    | Unlimited               | Trigger periodic prerequisite checks; timeout after 15 minutes.     |
| **Delayed Queue**    | Priority 4           | Preemptible                 | 10% of available CPU cycles           | 2 concurrent slots      | Auto-promote to Active Queue when scheduled execution time arrives. |
| **Dependency Queue** | Priority 5           | Preemptible                 | 10% of available CPU cycles           | 4 concurrent slots      | Auto-promote to Active Queue when dependency checks clear.          |

5.3 Scheduler Conflict Resolution and Deadlock Prevention

sequenceDiagram
    autonumber
    participant Sched as Task Scheduler
    participant ResA as System Resource A (Active File Lock)
    participant ResB as System Resource B (Web Session Lock)
    participant Task1 as Execution Task 1
    participant Task2 as Execution Task 2

    Task1->>ResA: Acquire Lock
    Task2->>ResB: Acquire Lock
    
    Note over Sched: Deadlock Risk Detected: Task 1 requests Resource B, Task 2 requests Resource A
    
    Task1->>ResB: Request Lock (Blocked)
    Task2->>ResA: Request Lock (Blocked)
    
    activate Sched
    Sched->>Sched: Run Cycle-Detection Audit (Tarjan's SCC Algorithm)
    Sched->>Task2: Issue Force-Preempt & Rollback Signal
    deactivate Sched
    
    Task2-->>ResB: Release Lock
    Task1->>ResB: Acquire Lock (Unblocked)
    Task1->>Task1: Execute Task Steps to Completion
    Task1-->>ResA: Release Lock
    Task1-->>ResB: Release Lock
    
    Task2->>ResB: Re-acquire Lock
    Task2->>ResA: Acquire Lock
    Task2->>Task2: Resume Execution

  - Priority Inversion Mitigation: If a Priority 2 task holds a resource lock
    required by a Priority 1 task, the scheduler temporarily upgrades the
    lower-priority task's status (using Priority Inheritance) to allow it to
    finish and release the lock.
  - Aging Formulas: Tasks waiting in low-priority queues have their urgency
    values adjusted over time to prevent starvation and ensure they eventually
    get processed:

\text{Urgency}_{new} = \text{Urgency}_{base} \times (1 + \lambda \cdot \Delta t_{waiting})

Where \lambda represents the queue aging coefficient and \Delta t_{waiting} is
the time spent waiting in the queue.

Section 6: Interrupt Controller

                                [INTERRUPT VECTOR TABLE]
                                           |
          +--------------------------------+--------------------------------+
          |                                |                                |
          v                                v                                v
   [Vector 0x00: Kill Switch]       [Vector 0x01: User Intervention] [Vector 0x02: Tool Exception]
   - Immediate shutdown             - Pauses active runs             - Triggers dynamic plan repair
   - Mask Level: Non-maskable       - Mask Level: Level 1            - Mask Level: Level 2

6.1 Interrupt Vector Table (IVT) Specifications

The Interrupt Controller coordinates incoming alerts, system anomalies, and user
interruptions, pausing execution threads safely when high-priority events are
detected.

6.2 CASPER Interrupt Vector Table Specification

| Interrupt Vector | Event Classification      | Source Trigger         | Mask Level             | Default Routing    | Target Recovery Action                                            |
| :--------------- | :------------------------ | :--------------------- | :--------------------- | :----------------- | :---------------------------------------------------------------- |
| **0x00**         | System Kill Switch        | Manual safety override | Non-maskable (Highest) | Emergency Pipeline | Immediate system shutdown, resource release, and state save.      |
| **0x01**         | User Command Interrupt    | User interface event   | Mask Level 1           | Focus Coordinator  | Pause running tasks, save context, and open active dialog window. |
| **0x02**         | Security Policy Violation | Policy audit engine    | Non-maskable           | Emergency Pipeline | Halt active tool execution, isolate worker, and notify user.      |
| **0x03**         | Financial Budget Breach   | Resource tracker       | Mask Level 2           | Execution Manager  | Suspend planning steps, save task state, and request approval.    |
| **0x04**         | Tool Process Crash        | Sandbox monitor        | Mask Level 3           | Scheduler Core     | Restart worker sandbox, rollback step, and retry task.            |
| **0x05**         | Network Connectivity Loss | System sensor          | Mask Level 4           | Delayed Queue      | Pause network tasks, move to waiting queue, and check ping.       |
| **0x06**         | Goal Deadline Alert       | Calendar agent         | Mask Level 5           | Active Queue       | Reorder task priority queues, promote deadline-sensitive tasks.   |
| **0x07**         | Background Sync Trigger   | System cron scheduler  | Mask Level 6 (Lowest)  | Background Queue   | Trigger database vacuuming and memory graph synchronization.      |

6.3 Nested Interrupt Routing

The Interrupt Controller supports Nested Interrupts, allowing high-priority
events to interrupt active lower-priority interrupt handlers:

sequenceDiagram
    autonumber
    participant Exec as Executive Kernel Core
    participant Int as Interrupt Controller
    participant LowHand as Low-Priority Handler (Vector 0x05)
    participant HighHand as High-Priority Handler (Vector 0x02)

    Exec->>Exec: Run Active Task Loop
    Note over Int: Event: Network connection lost (Vector 0x05)
    Int->>Exec: Preempt Task Execution
    Exec->>LowHand: Launch Recovery Routine
    
    activate LowHand
    Note over Int: Critical Event: Safety validation failed (Vector 0x02)
    
    Int->>LowHand: Preempt Low-Priority Handler
    LowHand->>LowHand: Save Progress State
    
    Exec->>HighHand: Launch Emergency Security Protocol
    activate HighHand
    HighHand->>HighHand: Run sandbox isolation and block outbound ports
    HighHand-->>Exec: Emergency Solved Message
    deactivate HighHand
    
    Exec-->>LowHand: Resume Progress
    LowHand->>LowHand: Recheck Connection Parameters
    LowHand-->>Exec: Return Success Result
    deactivate LowHand

Section 7: Context Switching

7.1 Task State Block (TSB) Structure

To allow tasks to be paused and resumed without losing progress, the Context
Manager stores active operational states in structured Task State Blocks (TSBs):

+-------------------------------------------------------------------------+
|                         TASK STATE BLOCK (TSB)                          |
+-------------------------------------------------------------------------+
|                                                                         |
|  [ CORE TASK METADATA ]                                                 |
|  Task ID: UUIDv4                                                        |
|  Parent Goal ID: UUIDv4                                                 |
|  Priority: 0-7                                                          |
|                                                                         |
|  [ WORKING MEMORY SCHEMA ]                                              |
|  Active Entities: List of UUIDs                                         |
|  Operational Variables: { Key: JSON }                                   |
|  Local Context Window: Metadata references                              |
|                                                                         |
|  [ EXECUTION STATE ]                                                    |
|  Active Plan Step ID: UUIDv4                                            |
|  Tool Registry Locks: List of IDs                                       |
|  Active Worker Port: PID / TCP Port                                     |
|                                                                         |
|  [ METRIC TRACKERS ]                                                    |
|  Accumulated Cost: USD                                                  |
|  Accumulated Token Usage: Integer                                       |
|  Execution Duration: milliseconds                                       |
|                                                                         |
+-------------------------------------------------------------------------+

7.2 Register-Free Context Saving Sequence

The Context Manager freezes and restores active task states using a structured
transition sequence, targeting context switches in under 10 milliseconds:

sequenceDiagram
    autonumber
    participant Exec as Executive Kernel Core
    participant Context as Context Manager
    participant Sched as Task Scheduler
    participant Worker as Active Execution Worker
    participant TSBStore as TSB Persistence Store

    Exec->>Sched: Trigger Preemption Command
    activate Sched
    Sched->>Worker: Issue Safe-Pause Event Signal
    activate Worker
    Worker->>Worker: Complete current atomic operation, hold resources
    Worker-->>Sched: Acknowledge Safe Pause
    deactivate Worker
    
    Sched->>Context: Save Task State Block (TSB)
    activate Context
    Context->>Context: Serialize variables, active entities, and token logs
    Context->>TSBStore: Commit TSB Snapshot File
    TSBStore-->>Context: Acknowledge Commit
    Context-->>Sched: Return Success Code
    deactivate Context
    
    Sched->>Exec: Switch Attention to Preempting Task
    deactivate Sched

Section 8: Goal Orchestration

                        [GOAL ORCHESTRATION ENGINE]
                                     |
         +---------------------------+---------------------------+
         |                                                       |
         v                                                       v
 [Acyclic Goal Lattice]                                  [Decomposition Loop]
  - Tracks goals and dependencies                         - Compiles HTNs
  - Resolves scheduling blocks                            - Validates step prerequisites

8.1 The Goal Lifecycle Lattice

The Goal Orchestration Engine coordinates user intents using a directed acyclic
Goal Lattice, mapping dependencies and managing state transitions across
projects:

stateDiagram-v2
    [*] --> Formulated : Parse User Intent
    Formulated --> Activated : Validate Constraints & Approve
    
    Activated --> Executing : Dispatch Active Tasks
    Activated --> Suspended : Attention Lock Preemption
    Suspended --> Activated : Focus Recovery Event
    
    Executing --> Blocked : Prerequisite Missing / Tool Exception
    Blocked --> Executing : Plan Repair Loop Success
    
    Executing --> Fulfilled : Goal Verification Complete
    Executing --> Aborted : Policy Violation / Safety Kill Event
    
    Blocked --> Aborted : Timeout Limit Reached
    
    Fulfilled --> [*]
    Aborted --> [*]

8.2 Hierarchical Task Network (HTN) Decomposition

To translate abstract goals into executable steps, the system uses an HTN
decomposition loop to break objectives down into structured task sequences:

flowchart TD
    A[Formulate High-Level Goal] --> B[Retrieve Domain Templates from Memory]
    B --> C[Validate Step Prerequisites]
    C --> D{Prerequisites Satisfied?}
    
    D -->|Yes| E[Compile Executable Plan steps]
    D -->|No| F[Insert Dependency Tasks into Queue]
    
    F --> C
    E --> G[Register Tasks in Task State Block]
    G --> H[Dispatch Tasks to Active Queue]

Section 9: Execution Manager

                                [EXECUTION MANAGER]
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
[Concurrency Slots]             [Worker Isolation Daemon]       [Transactional Rollback]
 - Manages active execution threads- Runs sandboxed processes       - Cleans environments on error
 - Enforces resource boundaries  - Isolates tool spaces          - Restores stable state backups

9.1 Concurrency Slot Allocation

The Execution Manager organizes task execution across isolated concurrency
slots, preventing resource conflicts and ensuring separate processes remain
sandboxed:

+-------------------------------------------------------------------------+
|                        CONCURRENCY SLOT REGISTRY                        |
+-------------------------------------------------------------------------+
|                                                                         |
|  [ SLOT 1: ACTIVE ]                                                     |
|  Task: Browser Scraping (Task ID: 9912)                                 |
|  Worker PID: 4012 | Port Allocation: 127.0.0.1:9001                     |
|                                                                         |
|  [ SLOT 2: ACTIVE ]                                                     |
|  Task: Workspace File Indexing (Task ID: 9913)                          |
|  Worker PID: 4015 | Port Allocation: 127.0.0.1:9002                     |
|                                                                         |
|  [ SLOT 3: VACANT ]                                                     |
|  State: Idle                                                            |
|  Port Allocation: 127.0.0.1:9003                                        |
|                                                                         |
|  [ SLOT 4: SYSTEM RESERVE ]                                             |
|  Reserved for Emergency Operations (Priority 0 Vector)                   |
|  Port Allocation: 127.0.0.1:9004                                        |
|                                                                         |
+-------------------------------------------------------------------------+

9.2 Execution Worker Life Cycle

Each execution worker runs within an isolated environment, managing its
lifecycle through structured operations:

stateDiagram-v2
    [*] --> Spawning : Initialize Sandbox
    Spawning --> Ready : Bind Communication Port
    Ready --> Running : Dispatch Step Payload
    
    Running --> Terminated : Execution Success / Return Result
    Running --> RollbackState : Timeout Exceeded / Exception Caught
    
    RollbackState --> Ready : Clean Sandbox Environment
    Ready --> [*] : Release Port & Terminate

9.3 Transactional Rollback Flow

If an execution step fails or is interrupted by a safety violation, the
Execution Manager rolls back environmental changes to return the system to its
last stable state:

flowchart TD
    A[Step Execution Failure / Policy Violation] --> B[Halt Active Worker Process]
    B --> C[Isolate Workspace & Revoke Environment Locks]
    C --> D[Retrieve Last Stable Backup Point from TSB]
    
    D --> E{Perform Environment Rollback}
    E -->|Database Updates| F[Rollback Transaction Logs]
    E -->|File Modifications| G[Restore Backup File Snapshots]
    E -->|External Actions| H[Issue Cancellation / Status Update Alerts]
    
    F & G & H --> I[Reset Worker State to Ready]
    I --> J[Notify Scheduler to Reschedule Task Step]

Section 10: Resource Allocation

10.1 Multi-Dimensional Resource Budgets

The Resource Manager monitors and throttles resource consumption using a
multi-dimensional budget system to ensure operations remain within safe and
cost-effective limits:

flowchart TD
    A[Inbound Task Step] --> B[Calculate Resource Demands]
    B --> C{Verify Budget Alignments}
    
    C -->|Within Budget Limits| D[Allocate Resource Tokens]
    C -->|Limit Exceeded| E[Halt Task & Notify User]
    
    D --> F[Active Execution Worker]
    F --> G[Log Token, Cost, CPU, & Time usage]
    G --> H[Update Resource Accounts]
    H --> B

10.2 Dynamic Resource Reallocation and Throttling

When system load is high or priority levels shift, the Resource Manager adjusts
allocations using a priority-weighting model:

\mathcal{C}_{allocated} = \mathcal{C}_{total} \times \frac{\omega_{task} \cdot \mathcal{P}_{task}}{\sum_{i=1}^{N} \omega_i \cdot \mathcal{P}_i}

Where:

  - \mathcal{C}_{total} is the total available system capacity (e.g., token
    limits or memory pools).
  - \mathcal{P}_{task} represents the priority level of the target task.
  - \omega_{task} is the priority allocation weight.
  - N is the count of active execution slots.

Under high loads, lower-priority background tasks are throttled to ensure
critical, user-facing processes have the resources needed to run efficiently.

10.3 Resource Limit & Budget Classifications

| Budget Target            | Base Limit          | Hard Limit       | Throttling Action                                         | Recovery Step                                       |
| :----------------------- | :------------------ | :--------------- | :-------------------------------------------------------- | :-------------------------------------------------- |
| **Token Budget**         | 20,000 tokens / run | 100,000 tokens   | Throttle model context length, drop system logs.          | Optimize prompt template, summarize history logs.   |
| **Memory Allocation**    | 512 MB RAM          | 2,048 MB RAM     | Limit worker process memory, suspend background indexing. | Clear memory caches, run database compaction.       |
| **API Costs ($)**        | $1.00 USD / run     | $10.00 USD / run | Suspend heavy model routing, fallback to local models.    | Seek user authorization, switch to low-cost models. |
| **Tool Execution Slots** | 3 active slots      | 8 active slots   | Queue new task requests, pause background checks.         | Terminate idle processes, release system ports.     |
| **Time Limits**          | 60 seconds / step   | 300 seconds      | Terminate execution worker, trigger step rollback.        | Reschedule task with optimized parameters.          |

Section 11: Executive State Machine

The Executive State Machine tracks and manages CASPER's overall operational
states, coordinating clean state transitions across tasks.

stateDiagram-v2
    [*] --> Idle : Initialize System Kernel
    
    Idle --> Observe : Trigger Event Input / Timer Signal
    Observe --> Attention : Evaluate Input Salience
    
    Attention --> Think : Urgent Signal / Attention Locked
    Attention --> Idle : Signal Masked / Low Urgency
    
    Think --> Plan : Goal Target Formulated
    Think --> Wait : Blocked on Context Requirements
    
    Plan --> Execute : Dispatch Active Task Lattices
    Plan --> Suspend : Context Preemption Triggered
    
    Execute --> Wait : Prerequisite Block / API Timeout
    Execute --> Complete : Goal Verification Success
    Execute --> Abort : Policy Violation / Safety Kill Event
    
    Wait --> Resume : Dependency Clear Event
    Wait --> Abort : Timeout Limit Reached
    
    Suspend --> Resume : Preempting Task Completed
    Resume --> Execute : Restore Task State Block
    
    Complete --> Idle
    Abort --> Idle

Section 12: Executive Communication Bus

+-----------------------------------------------------------------------------------+
|                         EXECUTIVE COMMUNICATION BUS                               |
+-----------------------------------------------------------------------------------+
|  +-----------------------------------------------------------------------------+  |
|  |                             Command Bus                                     |  |
|  |  [Active Commands] -> Executive Core -> Scheduler -> Task Dispatcher        |  |
|  +-----------------------------------------------------------------------------+  |
|                                         |                                         |
|                                         v                                         |
|  +-----------------------------------------------------------------------------+  |
|  |                              Event Bus                                      |  |
|  |  [System Events] -> Telemetry Monitors -> Resource Tracking -> Logging      |  |
|  +-----------------------------------------------------------------------------+  |
|                                         |                                         |
|                                         v                                         |
|  +-----------------------------------------------------------------------------+  |
|  |                             Response Bus                                    |  |
|  |  [Task Outputs] -> State Controller -> Memory Storage -> Core Interfaces     |  |
|  +-----------------------------------------------------------------------------+  |
+-----------------------------------------------------------------------------------+

12.1 Communication Channels

The Executive Communication Bus coordinates internal messaging across CASPER
using three dedicated, non-blocking channels:

  - Command Bus (Point-to-Point): Directs explicit instruction payloads from
    control plane managers to target execution slots, using strict serialization
    to ensure reliable delivery.
  - Event Bus (Publish/Subscribe): Streams real-time telemetry updates, status
    changes, and sensor readings to monitoring agents and system logs.
  - Response Bus (Asynchronous Stream): Returns output data and execution
    results from workers back to active coordination engines and memory stores.

12.2 Message Sequence Analysis

This sequence diagram shows how the Executive Communication Bus coordinates
messages during a routine task run:

sequenceDiagram
    autonumber
    participant Core as Executive Kernel Core
    participant Bus as Message Routing Bus
    participant Sched as Task Scheduler
    participant Worker as Active Execution Worker
    participant Mem as Memory Storage Registry

    Core->>Bus: Dispatch Command: Launch Task (Payload: Step 92)
    activate Bus
    Bus->>Sched: Route to Active Queue
    deactivate Bus
    
    activate Sched
    Sched->>Worker: Dispatch Task Run (Port Allocation)
    deactivate Sched
    
    activate Worker
    Worker->>Bus: Publish Event: Execution Started (Telemetry Stream)
    activate Bus
    Bus-->>Core: Update Focus HUD
    deactivate Bus
    
    Worker->>Worker: Execute task step actions
    Worker->>Bus: Dispatch Response: Task Completed (Payload: PDF Output)
    deactivate Worker
    
    activate Bus
    Bus->>Mem: Commit output files and update indexes
    Bus->>Core: Update task status logs
    deactivate Bus

Section 13: Execution Pipelines

                                [EXECUTION PIPELINES]
                                          |
          +-------------------------------+-------------------------------+
          |                               |                               |
          v                               v                               v
 [Simple Request Pipeline]       [Complex Goal Pipeline]         [Autonomous Run Pipeline]
  - Sub-100ms execution times     - Compiles HTN task trees       - Continuous execution loop
  - Minimal context window required- Manages multi-step dependencies- Runs scheduled background tasks

13.1 Pipeline Specifications & Operation Phases

1. Simple Request Pipeline (System 1)

Designed to process low-complexity, routine tasks (e.g., directory checks,
system status checks, short calculations) with sub-100ms latency:

  - Perception Phase: Receives a direct command input.
  - Triage Phase: Identifies the task as low-complexity and maps it to a
    pre-validated execution schema.
  - Execution Phase: Launches a fast-path worker thread to run the action,
    bypassing heavy planning loops.
  - Return Phase: Delivers the output directly and clears working memory.

2. Complex Goal Pipeline (System 2)

Manages multi-tier, variable tasks requiring deep coordination, planning, and
validation:

  - Perception Phase: Captures a high-level goal and retrieves relevant user
    preferences and historical logs.
  - Decomposition Phase: Compiles a Hierarchical Task Network (HTN) mapping step
    dependencies and resource budgets.
  - Verification Phase: The Policy Engine checks proposed steps against active
    safety boundaries.
  - Execution Phase: Dispatches tasks to sandboxed workers, monitoring progress
    and resource use across execution slots.
  - Reflection Phase: Reviews task results, records lessons learned, and updates
    the semantic knowledge graph.

3. Autonomous Execution Pipeline

Handles long-running, scheduled background tasks (e.g., continuous system
indexing, calendar organization, email categorization) during periods of low
active use:

  - Trigger Phase: The system cron scheduler or an event monitor starts the
    pipeline based on scheduled intervals or environmental triggers.
  - Scheduling Phase: Registers background tasks in low-priority queues,
    ensuring they yield to active user-facing processes.
  - Execution Phase: Runs tasks in background slots using restricted resource
    budgets.
  - Sync Phase: Synchronizes updates with local and cloud databases and goes
    back to sleep.

13.2 Execution Pipeline Phase Comparison

| Pipeline Type      | Latency Targets         | Concurrency Limit      | Default Queue Routing    | Safety Gate Threshold        | Active Context Size           |
| :----------------- | :---------------------- | :--------------------- | :----------------------- | :--------------------------- | :---------------------------- |
| **Simple Request** | $\leq 100$ milliseconds | Max 4 concurrent slots | Active Queue (Fast-path) | Minimal policy checks        | Small ($\leq 1,000$ tokens)   |
| **Complex Goal**   | Variable (1s - 30s)     | Max 2 concurrent slots | Active Queue             | High (Requires verification) | Medium ($\leq 16,000$ tokens) |
| **Background Run** | Variable (minutes)      | Max 1 concurrent slot  | Background Queue         | Moderate (Budget caps)       | Large ($\leq 32,000$ tokens)  |
| **Emergency**      | $\leq 10$ milliseconds  | Max 1 concurrent slot  | Emergency Queue          | Bypass normal scheduling     | Minimal ($\leq 500$ tokens)   |
| **Recovery**       | $\leq 50$ milliseconds  | Max 1 concurrent slot  | Emergency Queue          | Active policy checks         | Medium ($\leq 8,000$ tokens)  |

Section 14: Executive Safety

                                [EXECUTIVE SAFETY COGNITION]
                                             |
          +----------------------------------+----------------------------------+
          |                                  |                                  |
          v                                  v                                  v
   [Confirmation Gates]             [Execution Sandboxes]             [Circuit Breakers]
   - Interrupts high-risk runs      - Isolates worker environments     - Halts run errors
   - Enforces biometric validation  - Restricts folder access          - Prevents loop escalations

14.1 Multilayered Confirmation Gates

To protect user systems and data, the Policy Engine enforces strict,
multilayered confirmation gates for all high-risk operations:

flowchart TD
    A[Proposed Action Step] --> B{Analyze Risk Rating}
    
    B -->|High Risk: e.g., Payments, Deletions| C[Pause Task & Isolate Worker]
    B -->|Low Risk: e.g., Read file, Search| D[Authorize Execution Step]
    
    C --> E[Generate Native Confirmation Request Window]
    E --> F{User Authentication Success?}
    
    F -->|Yes: Biometric verified| D
    F -->|No: Timeout / Denied| G[Cancel Step, Rollback Changes, Log Warning]

14.2 Execution Sandboxing & System Boundaries

All third-party tools, browser sessions, and CLI scripts run in isolated, highly
restricted sandboxes to protect the host operating system:

  - Directory Restrictions: Workers are restricted to designated sandboxed
    folders and cannot access other filesystem directories unless authorized via
    secure macOS bookmark files.
  - Process Sandboxing: Code compilation and testing run within isolated
    container spaces, protecting the host system from potential exploits or
    crashes.
  - Network Throttling: Port access is restricted to verified target domains,
    blocking unauthorized local ports or unknown external connections.

14.3 Circuit Breakers and Fallbacks

The Executive Kernel uses real-time Circuit Breakers to identify, isolate, and
halt out-of-control execution paths before they strain system resources:

stateDiagram-v2
    [*] --> Closed : Normal Execution Monitoring
    
    Closed --> Open : Consecutive Failures > Threshold / Loop Detected
    state Open {
        [*] --> TerminateWorker
        TerminateWorker --> IsolateSandbox : Halt Process
        IsolateSandbox --> NotifyUser : Log Error Details
    }
    
    Open --> HalfOpen : Reset Signal / Safe Mode Triggered
    HalfOpen --> Closed : Verification Pass (Safe)
    HalfOpen --> Open : Verification Fail (Unstable)

Section 15: Performance Targets

15.1 Real-Time Performance Benchmarks

| Metric Target              | Target Value (Optimal) | Hard Upper Limit     | Measuring Protocol                                                         | Systems Impact                                     |
| :------------------------- | :--------------------- | :------------------- | :------------------------------------------------------------------------- | :------------------------------------------------- |
| **Scheduling Latency**     | $\leq 2$ milliseconds  | $5$ milliseconds     | Measures elapsed time from queue registration to task dispatch.            | Low latency ensures immediate task response.       |
| **Interrupt Latency**      | $\leq 5$ milliseconds  | $10$ milliseconds    | Measures elapsed time from event trigger to active thread preemption.      | Critical for immediate safety shutdowns.           |
| **Context Switch Latency** | $\leq 10$ milliseconds | $25$ milliseconds    | Measures elapsed time to freeze a TSB snapshot and load a new task.        | Smooth switches prevent interface lag.             |
| **Verification Latency**   | $\leq 5$ milliseconds  | $15$ milliseconds    | Measures time for the Policy Engine to check proposed steps against rules. | Low latency ensures fast, safe action validation.  |
| **Active Goal Capacity**   | $5$ concurrent goals   | $10$ active goals    | Tracks active goal lattice nodes concurrently in memory.                   | Prevents resource saturation during complex tasks. |
| **Concurrent Workers**     | $4$ active slots       | $8$ active slots     | Tracks active sandboxed worker processes in execution slots.               | Balances CPU load and prevents system lag.         |
| **Max Browser Sessions**   | $2$ active instances   | $4$ active instances | Tracks active automated chromium browser instances in execution.           | Restricts memory consumption on user devices.      |

15.2 Scaling and Efficiency Assumptions

To operate efficiently on standard personal computers, CASPER runs with a
lightweight footprint, using minimal idle resources and scaling up processing
capacity only when active, high-priority tasks are dispatched.

                  [RESOURCE SCALING PATTERNS]
                               |
       +-----------------------+-----------------------+
       |                                               |
       v                                               v
[IDLE MONITORS RUN]                             [ACTIVE TASK ENGAGED]
 - CPU Utilization: < 1.0%                       - CPU Utilization: Up to 40% (Scaled)
 - Memory Allocation: < 150 MB RAM               - Memory Allocation: Up to 1.5 GB RAM

Section 16: Failure Recovery

                             [FAILURE RECOVERY CORE]
                                        |
         +------------------------------+------------------------------+
         |                              |                              |
         v                              v                              v
[Kernel Panic Daemon]          [Infinite Loop Monitor]        [Corruption Auditing Engine]
 - Identifies subsystem crashes - Tracks redundant iterations   - Scans serialized TSB files
 - Reboots isolated structures  - Restores last safe context   - Rebuilds context from logs

16.1 Automated Recovery Workflows

1. Executive Kernel Crash

  - Failure State: A core kernel subsystem becomes unresponsive or throws a
    fatal runtime exception.
  - Recovery Action: The system watchdog daemon isolates the crashed subsystem,
    freezes running task queues, re-initializes the kernel core, restores active
    task states from saved TSB snapshots, and resumes operations.

2. Infinite Reasoning Loop

  - Failure State: An agent becomes stuck in a repeating cycle of similar
    reasoning steps without executing actions or showing progress.
  - Recovery Action: The Metacognition monitor detects the repeating pattern,
    pauses the planning thread, runs a task-tree optimization sweep to resolve
    inconsistencies, and prompts the user for direction if the loop continues.

3. Memory File Corruption

  - Failure State: A serialized TSB or active context block becomes unreadable
    or corrupt due to unexpected write errors.
  - Recovery Action: The Context Manager rejects the corrupted block, retrieves
    the last safe checkpoint file from the local backup log, reads previous step
    records to verify parameters, and continues task execution.

16.2 Crash Recovery Sequence Analysis

This sequence diagram shows how the watchdog daemon restores system operations
after an unexpected crash:

sequenceDiagram
    autonumber
    participant Watch as System Watchdog Daemon
    participant Core as Executive Kernel Core
    participant Context as Context Manager
    participant TSBStore as TSB Persistence Store
    participant Sched as Task Scheduler

    Core->>Core: Crash Event: Unresponsive Loop Detected
    Watch->>Core: Health Ping (Timeout Exceeded)
    
    activate Watch
    Watch->>Core: Issue Force-Kill Signal
    Watch->>Context: Query Last Stable Checkpoint File
    activate Context
    Context->>TSBStore: Retrieve Verified TSB Snapshots
    TSBStore-->>Context: Return TSB Payload
    Context-->>Watch: Return Stable Task State
    deactivate Context
    
    Watch->>Core: Initialize New Kernel Core
    activate Core
    Core-->>Watch: System Ready Alert
    deactivate Core
    
    Watch->>Sched: Restore Task State Block & Resume Active Queues
    activate Sched
    Sched->>Sched: Reprioritize queues, dispatch step actions to workers
    Sched-->>Watch: Recovery Operations Complete
    deactivate Sched
    deactivate Watch

Section 17: Observability

                                [SYSTEM OBSERVABILITY]
                                           |
          +--------------------------------+--------------------------------+
          |                                |                                |
          v                                v                                v
   [Distributed Tracing]            [Telemetry Collectors]           [Queue Monitors]
   - Generates trace identifiers    - Aggregates resource profiles   - Tracks active queue counts
   - Tracks paths across agents     - Logs CPU, RAM, and token usage - Flags bottlenecked queues

17.1 Telemetry Aggregation Specs

To maintain complete operational visibility, CASPER tracks performance metrics
and resource usage across all cognitive layers:

  - Unified Trace Identifiers: A trace ID follows execution flows from raw user
    input through planning and tool execution down to database logs, simplifying
    system troubleshooting.
  - Resource Profiles: The monitoring engine logs CPU allocations, RAM usage,
    model tokens, and API transaction costs to identify performance bottlenecks.
  - Queue Tracking: Monitors queue counts, waiting times, and execution
    latencies across active queues, flagging bottlenecks before they impact
    performance.

17.2 Structured Observability Schema

+-------------------------------------------------------------------------+
|                       SYSTEM OBSERVABILITY EVENT                        |
+-------------------------------------------------------------------------+
|                                                                         |
|  [ EVENT IDENTIFICATION ]                                               |
|  Event ID: UUIDv4                                                       |
|  Trace ID: UUIDv4                                                       |
|  Timestamp: ISO 8601 Nanoseconds                                        |
|                                                                         |
|  [ ORIGIN INFO ]                                                        |
|  Service Name: "casper-execution-manager"                               |
|  Subsystem: "worker-pool"                                               |
|                                                                         |
|  [ METRIC PAYLOAD ]                                                     |
|  CPU Utilization: float                                                 |
|  RAM Consumption: bytes                                                 |
|  Token Count: integer                                                   |
|  Execution Duration: milliseconds                                       |
|                                                                         |
|  [ CONTEXT KEYS ]                                                       |
|  Active Task ID: UUIDv4                                                 |
|  Active Goal ID: UUIDv4                                                 |
|  Execution Slot: 0-7                                                    |
|                                                                         |
+-------------------------------------------------------------------------+

Section 18: Distributed Executive

                             [DISTRIBUTED EXECUTIVE MESH]
                                          |
         +--------------------------------+--------------------------------+
         |                                |                                |
         v                                v                                v
 [Raft Consensus Router]         [State Replication Bus]          [Distributed Scheduler]
  - Elects active leader node     - Syncs database changes         - Balances tasks across devices
  - Manages node configurations   - Coordinates memory graphs      - Routes runs to local / cloud

18.1 Multi-Device Synchronization

CASPER can run across multiple user devices (e.g., Desktop, Phone, Cloud Node)
using a distributed, decentralized mesh network:

  - Consensus-Driven Leadership (Raft): The device cluster runs Raft consensus
    algorithms to elect an active leader node to coordinate scheduling, while
    other nodes act as local processing resources.
  - State Replication Bus: Changes to the user's semantic memory, task
    histories, and system settings are encrypted and synchronized across devices
    using secure, conflict-free data channels.
  - Dynamic Mesh Scheduling: The leader node balances processing tasks across
    available devices, routing heavy simulation tasks to cloud nodes while
    running low-latency voice and interface tasks locally.

18.2 Distributed Execution Flow

This flowchart shows how the Distributed Executive balances tasks across
different user devices within the local mesh network:

flowchart TD
    A[Inbound Task Request] --> B[Distributed Scheduler]
    B --> C{Verify Node Capabilities}
    
    C -->|High Compute: e.g., Heavy planning| D[Route Task to Cloud Node / Local Server]
    C -->|Low Latency: e.g., Audio capture| E[Route Task to Phone / Local Edge Device]
    C -->|Standard Work: e.g., Browsing| F[Route Task to Desktop Host Node]
    
    D & E & F --> G[Execute Task Step on Target Node]
    G --> H[Sync Execution Logs with Distributed State Store]
    H --> I[Update Unified Memory Graph across Mesh Nodes]

Section 19: Executive Evolution

19.1 Architectural Upgrade Path

timeline
    title Executive Scheduler Milestones
    Phase 1 : Static Rule-Based Schedulers : Structured multi-level queues : Manual interrupt vectors
    Phase 2 : Adaptive Schedulers : Priority aging algorithms : Dynamic resource reallocation under load
    Phase 3 : Neural Cognitive Schedulers : Predictive context allocation : Autonomous tool slot scaling

  - Adaptive Resource Allocations: Next-generation schedulers will analyze
    historical task runs to anticipate resource demands and pre-allocate context
    and tokens before tasks start.
  - Self-Optimizing Queues: Future systems will automatically adapt scheduling
    priorities and throttling profiles to match the user's specific workflow
    patterns and habits.
  - Predictive Preemption: Schedulers will predict potential service timeouts
    and resource bottlenecks, proactively pausing or re-routing tasks to avoid
    performance drops.

Section 20: Complete Executive Flow

This flowchart illustrates CASPER's complete, end-to-end executive execution
flow, tracing a user request from raw perception, through salience gating,
scheduling, planning, execution, and subsequent learning updates.

flowchart TD
    %% Sensory Input & Triage
    A[Raw User Request / Input Event] --> B[Sensory Layer Perception]
    B --> C[Attention Controller Salience Filter]
    
    %% Salience Gate
    C --> D{Salience Threshold Cleared?}
    D -->|No| E[Log to Background Queue]
    D -->|Yes| F{Attention Lock Active?}
    
    %% Attention Locking Check
    F -->|Yes| G{Interrupt Priority > Lock Level?}
    F -->|No| H[Trigger Preemption Context Switch]
    
    G -->|No| E
    G -->|Yes| H
    
    %% Context Management & Scheduling
    H --> I[Context Manager: Save Current Task TSB]
    I --> J[State Controller: Set Active Focus Target]
    J --> K[Task Scheduler: Route Task to Priority Queue]
    
    %% Goal Processing & Planning
    K --> L[Goal Orchestration Engine: Validate Goal Parameters]
    L --> M[Planning Engine: Compile HTN Task Lattice]
    M --> N[Policy Engine: Safety Verification Audit]
    
    %% Safety Gateway Check
    N --> O{Security Constraints Cleared?}
    O -->|No| P[Isolate Workspace, Rollback Changes, Log Warning]
    O -->|Yes| Q{Verify Risk Level}
    
    %% Risk Confirmation Gates
    Q -->|High Risk| R[Pause Task & Request Biometric Approval]
    Q -->|Low Risk| S[Execution Manager: Allocate Concurrency Slot]
    
    R -->|User Approved| S
    R -->|User Denied| P
    
    %% Worker Dispatch & Monitoring
    S --> T[Spawn Sandboxed Execution Worker]
    T --> U[Skills & Actuators Run Task Steps]
    U --> V[Metacognitive Monitoring Loop]
    
    %% Execution Verification Loop
    V --> W{Step Execution Success?}
    W -->|No| X[Pause Task, Trigger Dynamic Plan Repair]
    X --> K
    
    %% Post-Task Learning & Updates
    W -->|Yes| Y[Reflection Engine: Run Post-Mortem Review]
    Y --> Z[Learning Engine: Refine Procedural Schemas]
    Z --> AA[Update Memory Graph & Save TSB Checkpoint]
    
    %% Loop Reset
    AA --> AB[Clear Working Memory & Resume Scheduled Queues]
    AB --> B

20.1 Core Cognitive Flow State Transitions

| Step   | Processing Module     | Target Action                | Core Logic                                      | Performance Target      |
| :----- | :-------------------- | :--------------------------- | :---------------------------------------------- | :---------------------- |
| **1**  | Attention Controller  | Gating environmental inputs. | Compares input salience with focus locks.       | $\leq 2$ milliseconds   |
| **2**  | Context Manager       | Saving paused task states.   | Serializes active variables to a TSB snapshot.  | $\leq 10$ milliseconds  |
| **3**  | Task Scheduler        | Routing tasks to priorities. | Places tasks in matching queue tiers.           | $\leq 2$ milliseconds   |
| **4**  | Goal Orchestration    | Decomposing goal structures. | Breaks high-level goals into HTN plans.         | Variable (10ms - 200ms) |
| **5**  | Policy Engine         | Running safety audits.       | Checks proposed actions against safety rules.   | $\leq 5$ milliseconds   |
| **6**  | Execution Manager     | Spawning process workers.    | Launches sandboxed task execution slots.        | $\leq 5$ milliseconds   |
| **7**  | Skills & Actuators    | Running plan steps.          | Performs physical web or system actions.        | Variable (seconds)      |
| **8**  | Metacognitive Monitor | Checking execution metrics.  | Triggers circuit breakers if errors accumulate. | Continuous              |
| **9**  | Reflection Engine     | Analyzing task performance.  | Compares results to targets to find errors.     | Post-task background    |
| **10** | Learning Engine       | Updating system parameters.  | Integrates lessons learned into memory graphs.  | Nightly consolidation   |

