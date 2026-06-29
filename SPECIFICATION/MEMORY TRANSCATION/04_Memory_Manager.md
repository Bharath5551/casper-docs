CASPER: Volume 4 — Memory System Specification

Version 2.0.0-MEM

Classification: Restricted — Cognitive Memory Engineering Division

Part I — Memory Philosophy

1.1 The Necessity of Memory in Active Inference

Autonomous cognitive systems must operate within partially observable Markov
decision processes (POMDPs). A stateless model acts only on immediate
observations, which leads to unstable decisions, repetitive loops, and an
inability to track multi-step goals.

Memory is the system's mechanism for tracking time and state. In the framework
of Active Inference and the Free Energy Principle, memory serves as the prior
distribution. It represents the system's current beliefs about the world,
constructed from past experiences:

\mathbf{F} = \sum_{\tau} \mathbf{E}_{q(s_\tau | o_\tau)} [\ln q(s_\tau | o_\tau) - \ln p(o_\tau, s_\tau)]

Where:

  - q(s_\tau | o_\tau) is the variational posterior belief of the state s_\tau
    given sensory observations o_\tau.
  - p(o_\tau, s_\tau) is the generative model representing the joint probability
    of observations and states.

Without a persistent, structured memory, the generative model cannot calculate
predictive error over time, causing the system to drift and fail.

                    [SENSORY INFLOW] (o_t)
                           |
                           v
              +-------------------------+
              |   Active Inference      | <---+ [ACTIVE MARKOV STATE]
              |   Error Minimization    |     | (s_t, updated via priors)
              +-------------------------+     |
                           |                  |
                           v                  |
              +-------------------------+     |
              |   Variational Posterior | ----+
              |   Belief Synthesis      |
              +-------------------------+
                           |
                           v
              +-------------------------+
              |     Action Selection    |
              |     (Policy Dispatch)   |
              +-------------------------+

1.2 Cognitive Memory vs. Systems Storage

+-----------------------------------------------------------------------------------+
|                            COGNITIVE MEMORY HIERARCHY                             |
+-----------------------------------------------------------------------------------+
|  +-----------------------------------------------------------------------------+  |
|  |                           LLM Context Window                                |  |
|  |  - In-flight attention keys & values (Transient CPU Cache)                  |  |
|  +-----------------------------------------------------------------------------+  |
|                                         |                                         |
|                                         v                                         |
|  +-----------------------------------------------------------------------------+  |
|  |                            Working Memory                                   |  |
|  |  - Active task entities, variables, and goal state blocks (SRAM)            |  |
|  +-----------------------------------------------------------------------------+  |
|                                         |                                         |
|                                         v                                         |
|  +-----------------------------------------------------------------------------+  |
|  |                           Persistent Memory                                 |  |
|  |  - Transaction logs, key-value variables, and config profiles (SSD)         |  |
|  +-----------------------------------------------------------------------------+  |
|                                         |                                         |
|                                         v                                         |
|  +-----------------------------------------------------------------------------+  |
|  |                           Cognitive Memory                                  |  |
|  |  - Multi-tiered associative, episodic, and semantic graphs (Virtual Storage) |  |
|  +-----------------------------------------------------------------------------+  |
+-----------------------------------------------------------------------------------+

  - LLM Context: Analogous to CPU registers or L1 cache. It stores the active
    attention keys and values (K, V matrices) for the current inference cycle,
    but is completely cleared when the session terminates.
  - Working Memory: Analogous to system SRAM. It holds the active task
    variables, current goal parameters, and immediate environmental cues needed
    to run active processes.
  - Persistent Memory: Analogous to local disk storage (SSD). It provides
    reliable transactional persistence for logs, active variables, and system
    configurations.
  - Cognitive Memory: A multi-tiered, associative memory fabric. It structures
    experiences into episodic journals and maps entities and concepts into a
    semantic knowledge graph, serving as the system's long-term intelligence
    base.

1.3 Memory as a First-Class System Service

In CASPER, memory is not a passive database wrapper. It is designed as a
first-class, active systems service:

  - Active Consolidation: Runs background summarization and indexing tasks
    during low-activity periods to compact raw logs into semantic structures.
  - Dynamic Eviction & Forgetting: Employs decay algorithms to prune low-value
    or redundant data, keeping memory pools optimized.
  - Inline Policy Enforcement: Evaluates all read and write commands against
    security and privacy rules before data is accessed.

Part II — Complete Memory Architecture

+--------------------------------------------------------------------------------------------------+
|                                    COMPLETE MEMORY SYSTEM                                        |
+--------------------------------------------------------------------------------------------------+
|  +---------------------------+   +---------------------------+   +----------------------------+  |
|  |      Sensory Buffer       |-->|      Working Memory       |-->|     Active Context L2      |  |
|  | (Raw audio, video, pings) |   | (Variables, active TSBs)  |   | (Segmented focus window)   |  |
|  +---------------------------+   +---------------------------+   +----------------------------+  |
|                                                |                                |                |
|                                                v                                v                |
|  +-----------------------------------------------------------+   +----------------------------+  |
|  |                    Episodic Journal L3                    |-->|     Semantic Graph L4      |  |
|  | (Chronological logs, run histories, execution traces)     |   | (Entities, concepts, rules)|  |
|  +-----------------------------------------------------------+   +----------------------------+  |
|                                |                                                |                |
|                                v                                                v                |
|  +--------------------------------------------------------------------------------------------+  |
|  |                                  Long-Term Archive L5                                      |  |
|  |                  (Compressed history files, offline backup packages)                       |  |
|  +--------------------------------------------------------------------------------------------+  |
+--------------------------------------------------------------------------------------------------+

2.1 Complete Memory System Classification Matrix

The CASPER Memory System manages 36 distinct memory pools, each optimized for
specific data lifetimes, backends, and performance profiles.

| Memory Class            | Lifetime                | Ownership              | Storage Backend       | Data Schema             | Index Strategy           | Eviction Policy        |
| :---------------------- | :---------------------- | :--------------------- | :-------------------- | :---------------------- | :----------------------- | :--------------------- |
| **Sensory Buffer**      | Real-time ($\leq 5$s)   | Perceptual Input Layer | RAM Ring Buffer       | Raw Binary Frame        | Temporal Offset Index    | Oldest Overwrite       |
| **Working Memory**      | Active Task Cycle       | Executive Core         | In-Memory Key-Value   | Task State Block        | Hash Table Lookup        | Task Complete Purge    |
| **Active Context**      | Run Session             | Context Engine         | RAM Pages             | Context Window JSON     | Inverted Metadata Index  | Session Clear          |
| **Episodic Memory**     | Long-Term (Permanent)   | Memory Engine          | SQLite Database       | Chronicle Log Row       | Vector + Temporal HNSW   | Compression Archive    |
| **Semantic Memory**     | Long-Term (Permanent)   | Knowledge Graph        | Neo4j Graph DB        | Entity Node Schema      | B-Tree Edge Lookup       | Pruning Optimization   |
| **Procedural Memory**   | Permanent               | Skills Registry        | In-Memory Key-Value   | Tool Input JSON         | capability Index         | Signature Lock         |
| **Short-Term Memory**   | Up to 24 Hours          | Memory Engine          | SQLite Database       | Rolling Event Row       | Temporal Index           | Consolidation Sweep    |
| **Reflection Memory**   | Long-Term (Permanent)   | Reflection Engine      | SQLite + Neo4j        | Lesson Learned Schema   | Vector Similarity        | Compression Archive    |
| **Planning Memory**     | Active Planning Run     | Planning Engine        | RAM Trees             | HTN Lattices            | Plan Path Tree           | Plan Completion Purge  |
| **Goal Memory**         | Project Duration        | Goal Engine            | Neo4j Graph DB        | Goal State Node         | Acyclic Dependency Graph | Goal Fulfillment Purge |
| **Conversation Memory** | Permanent               | Conversation Engine    | SQLite Database       | Dialog Turn Schema      | Vector + Temporal Index  | Compression Archive    |
| **Execution Memory**    | Task Run Duration       | Execution Manager      | SQLite Database       | Step Execution Log      | UUID Hash Index          | Task Complete Purge    |
| **Tool Memory**         | Permanent               | Tool Registry          | JSON Manifest         | Tool Capability Schema  | Keyword Directory        | Package Uninstall      |
| **Plugin Memory**       | Permanent               | Plugin SDK             | WASM File Storage     | Manifest JSON           | Capability Directory     | Plugin Uninstall       |
| **World Model Memory**  | Long-Term (Permanent)   | World Model Engine     | Neo4j Graph DB        | Entity State Node       | Spatial + Temporal Index | Pruning Optimization   |
| **Spatial Memory**      | Permanent               | World Model Engine     | SQLite Database       | Location Coordinate Row | R-Tree Coordinate Index  | Pruning Optimization   |
| **Temporal Memory**     | Permanent               | Memory Engine          | SQLite Database       | Event Timeline Row      | Temporal Timestamp Index | Pruning Optimization   |
| **Relationship Memory** | Permanent               | Conversation Engine    | Neo4j Graph DB        | Trust Attribute Node    | Graph Edge Traversal     | Manual User Purge      |
| **Identity Memory**     | Permanent               | Identity Layer         | Immutable Config File | Core Axiom Schema       | Cryptographic Key Lookup | Locked (No Eviction)   |
| **Skill Memory**        | Permanent               | Learning Engine        | WASM File Storage     | Skill Manifest Schema   | Keyword Directory        | Package Uninstall      |
| **Vector Memory**       | Permanent               | Memory Engine          | Qdrant Vector DB      | High-Dim Payload        | Cosine Similarity HNSW   | Pruning Optimization   |
| **Cache Memory**        | Ephemeral ($\leq 1$ hr) | Memory Manager         | RAM Key-Value         | Temporary Cache Chunk   | LRU Hash Index           | Least Recently Used    |
| **Checkpoint Memory**   | Permanent               | Checkpoint System      | Incremental File Snap | Unified Snapshot Schema | State Version Tree       | Pruning Optimization   |
| **Archive Memory**      | Permanent               | Memory Manager         | Compressed Disk File  | Archived Chunk Tarball  | Archive Metadata Index   | Offsite Replication    |
| **Metadata Memory**     | Permanent               | Memory Engine          | SQLite Database       | Object Metadata Row     | Multi-Column Index       | Parent Record Deletion |
| **Context Memory**      | Active Task Cycle       | Context Engine         | RAM Pages             | Context Window Schema   | Inverted Metadata Index  | Task Complete Purge    |
| **Registry Memory**     | Permanent               | Tool Registry          | SQLite Database       | Registration Registry   | UUID Hash Index          | Uninstall Purge        |
| **Execution Graph**     | Task Run Duration       | Execution Manager      | SQLite Database       | Execution Path Log      | Parent Task Index        | Task Complete Purge    |
| **Failure Memory**      | Permanent               | Reflection Engine      | SQLite Database       | Failure Log Row         | Vector Similarity        | Pruning Optimization   |
| **Simulation Memory**   | Run Session             | Reasoning Engine       | RAM Key-Value         | Simulation Path JSON    | Acyclic Path Tree        | Session Clear          |
| **Prediction Memory**   | Permanent               | Prediction System      | SQLite Database       | Prediction Event Row    | Temporal Target Index    | Target Date Expiration |
| **Policy Memory**       | Permanent               | Policy Engine          | SQLite Database       | Verification Rule Row   | B-Tree Rule Index        | Rule Override Purge    |
| **Preference Memory**   | Permanent               | Identity Layer         | SQLite Database       | Preference Node Schema  | User Attribute Index     | Manual User Purge      |
| **Security Memory**     | Permanent               | Security Manager       | Encrypted Keyring     | Encrypted Key Envelope  | Cryptographic Key Lookup | Locked (No Eviction)   |
| **Budget Memory**       | Run Session             | Resource Manager       | RAM Key-Value         | Budget Balance Schema   | Slot Index Lookup        | Session Clear          |
| **Telemetry Memory**    | Rolling 30 Days         | Monitoring System      | SQLite Database       | Telemetry Metric Row    | Temporal Timestamp Index | 30-Day TTL Deletion    |

Part III — Memory Hierarchy

CASPER structures its memory systems across five hierarchical tiers, balancing
high-speed working caches with compressed, long-term semantic archives:

+--------------------------------------------------------------------------------------------------+
|                                    MEMORY HIERARCHY PROFILE                                      |
+--------------------------------------------------------------------------------------------------+
|                                                                                                  |
|  [ L0 Sensory Buffer ] ------------------------------> Latency: < 1ms | Capacity: 10 MB          |
|  - Real-time video frames and audio streams                                                      |
|                                                                                                  |
|  [ L1 Working Memory ] ------------------------------> Latency: < 2ms | Capacity: 100 MB         |
|  - Active Task State Blocks (TSBs) and variables                                                 |
|                                                                                                  |
|  [ L2 Active Context ] ------------------------------> Latency: < 10ms | Capacity: 1 GB          |
|  - Active prompt assembly contexts and indices                                                   |
|                                                                                                  |
|  [ L3 Episodic Memory ] -----------------------------> Latency: < 50ms | Capacity: 100 GB         |
|  - Structured run histories and event logs                                                       |
|                                                                                                  |
|  [ L4 Semantic Memory ] -----------------------------> Latency: < 100ms | Capacity: 500 GB        |
|  - Inter-linked entity-relationship knowledge graphs                                             |
|                                                                                                  |
|  [ L5 Long-Term Archive ] ---------------------------> Latency: < 1000ms | Capacity: Unlimited    |
|  - Highly-compressed, encrypted historical archives                                              |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+

3.1 Virtual Cognitive Paging & Compaction

flowchart TD
    A[Sensory Buffer L0] --> B[Working Memory L1: Serialize TSBs]
    B --> C{Verify Context Page Allocations}
    
    C -->|Within active limits| D[Active Context L2]
    C -->|Limit Exceeded| E[Paging Engine: Page to Episodic L3]
    
    E --> F[Episodic Memory L3 Database Writes]
    F --> G[Nightly Consolidation Sweep]
    G --> H[Semantic Integration & Graph Update L4]
    H --> I[Compress to Long-Term Archive L5]

  - Cognitive Paging: Active context blocks are structured as dynamic virtual
    pages. When the active context exceeds L2 memory limits, the Paging Engine
    writes older, less relevant pages to L3 Episodic storage, freeing up fast L2
    memory.
  - Semantic Consolidation: During nightly maintenance periods, episodic memory
    records are compiled, summarized, and integrated into the L4 Semantic
    knowledge graph, compressing raw execution logs into key concept
    relationships.
  - Archival Compression: Historical data that has not been retrieved within a
    defined time window is packed into encrypted, compressed packages, reducing
    local storage footprints.

Part IV — Memory Manager

                                  [MEMORY MANAGER]
                                          |
          +-------------------------------+-------------------------------+
          |                               |                               |
          v                               v                               v
 [Arena Allocator Core]         [Memory Reference Counter]      [Memory Pressure Monitor]
  - Allocates fast context pages  - Tracks active memory hooks    - Monitors system memory strain
  - Enforces page limits          - Clears unreferenced blocks    - Triggers page evictions

The Memory Manager coordinates internal memory page allocations, object
lifetimes, and garbage collection, preventing resource leaks and ensuring fast
memory access.

4.1 Memory Manager Subsystems Specification

| Subsystem             | Responsibilities                                                             | Core Interfaces                        | Inputs                            | Outputs                    | Performance Targets            | Failure Modes                    | Recovery Protocol                                              |
| :-------------------- | :--------------------------------------------------------------------------- | :------------------------------------- | :-------------------------------- | :------------------------- | :----------------------------- | :------------------------------- | :------------------------------------------------------------- |
| **Allocator**         | Allocates context pages, manages free lists and memory arenas.               | `allocate_page()`, `free_page()`       | Memory allocation requests, sizes | Allocated memory pointers  | Allocation latency under 1ms   | Memory fragmentation             | Run page compaction and defragmentation sweeps                 |
| **Free Lists**        | Tracks available virtual memory pages and blocks.                            | `get_free_block()`, `release_block()`  | Block release commands, metadata  | Free memory blocks         | Lookup under 0.1ms             | List pointer corruption          | Rebuild free lists, verify memory map indexes                  |
| **Arena Allocator**   | Allocates blocks within contiguous memory arenas for task execution.         | `create_arena()`, `destroy_arena()`    | Arena size parameters, task IDs   | Arena pointers             | Allocation under 0.1ms         | Arena boundary overrun           | Halts task, isolate arena, reallocate with larger limits       |
| **Object Pools**      | Pre-allocates memory for highly repeating task objects (e.g., Event, Chunk). | `acquire_object()`, `release_object()` | Object configurations             | Reusable object references | Retrieval under 0.1ms          | Pool exhaustion error            | Scale pool allocations dynamically, trigger garbage collection |
| **Garbage Collector** | Scans memory blocks, cleans unreferenced variables and caches.               | `scan_heap()`, `sweep_unreferenced()`  | Active pointer indexes            | Freed memory blocks        | Sweep complete under 10ms      | Mark-and-sweep loop freeze       | Force-halt collector, run targeted thread cleanup              |
| **Reference Counter** | Tracks active hooks to memory variables and context blocks.                  | `increment_ref()`, `decrement_ref()`   | Variable identifiers              | Reference counts           | Counter update under 0.1ms     | Circular reference deadlock      | Run depth-first dependency audits, clear unreferenced cycles   |
| **Compactor**         | Compacts context pages, resolving gaps and reducing memory footprints.       | `compact_heap()`, `relocate_page()`    | Active memory blocks map          | Realigned memory map       | Compaction complete under 50ms | Relocation address pointer fault | Restore address maps from transaction log, rollback page       |
| **Pressure Monitor**  | Monitors system memory strain, triggering page evictions under load.         | `get_pressure()`, `evict_page()`       | Host OS memory logs               | Eviction commands          | Detection latency under 1ms    | Threshold failure under load     | Evict all temporary caches, halt low-priority background tasks |

Part V — Memory Objects

Memory objects in CASPER are defined as verifiable data structures, ensuring
consistent schemas and fast, reliable serialization across the system.

+--------------------------------------------------------------------------------------------------+
|                                    MEMORY OBJECT SCHEMAS                                         |
+--------------------------------------------------------------------------------------------------+
|                                                                                                  |
|     +------------------+                 +--------------------+                 +------------+   |
|     |  Goal Object     |                 |  Context Object    |                 | Task Object|   |
|     |  - Goal ID       |                 |  - Context ID      |                 | - Task ID  |   |
|     |  - User ID       |                 |  - Variables Map   |                 | - Priority |   |
|     +------------------+                 +--------------------+                 +------------+   |
|              |                                     |                                   |         |
|              v                                     v                                   v         |
|     +----------------------------------------------------------------------------------------+   |
|     |                                   SERIALIZATION ENGINE                                 |   |
|     |                 Formats data payloads into clean binary / JSON structures              |   |
|     +----------------------------------------------------------------------------------------+   |
|                                                    |                                             |
|                                                    v                                             |
|                                           +------------------+                                   |
|                                           | Persistent Storage|                                   |
|                                           | - Local DB Files |                                   |
|                                           | - Memory Caches  |                                   |
|                                           +------------------+                                   |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+

1. Goal Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "GoalObject",
  "type": "object",
  "required": ["goal_id", "user_id", "statement", "priority", "state", "dependencies", "milestones"],
  "properties": {
    "goal_id": { "type": "string", "format": "uuid" },
    "user_id": { "type": "string", "format": "uuid" },
    "statement": { "type": "string" },
    "priority": { "type": "integer", "minimum": 0, "maximum": 7 },
    "state": { "type": "string", "enum": ["FORMULATED", "ACTIVE", "SUSPENDED", "FULFILLED", "ABORTED"] },
    "dependencies": {
      "type": "array",
      "items": { "type": "string", "format": "uuid" }
    },
    "milestones": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["milestone_id", "description", "is_completed"],
        "properties": {
          "milestone_id": { "type": "string", "format": "uuid" },
          "description": { "type": "string" },
          "is_completed": { "type": "boolean" }
        }
      }
    }
  }
}

2. Context Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ContextObject",
  "type": "object",
  "required": ["context_id", "variables", "active_entities", "memory_pointers"],
  "properties": {
    "context_id": { "type": "string", "format": "uuid" },
    "variables": {
      "type": "object",
      "additionalProperties": { "type": "string" }
    },
    "active_entities": {
      "type": "array",
      "items": { "type": "string", "format": "uuid" }
    },
    "memory_pointers": {
      "type": "array",
      "items": { "type": "string" }
    }
  }
}

3. Conversation Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ConversationObject",
  "type": "object",
  "required": ["session_id", "user_id", "turns", "summary"],
  "properties": {
    "session_id": { "type": "string", "format": "uuid" },
    "user_id": { "type": "string", "format": "uuid" },
    "turns": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["turn_id", "speaker", "content", "timestamp"],
        "properties": {
          "turn_id": { "type": "string", "format": "uuid" },
          "speaker": { "type": "string", "enum": ["USER", "CASPER"] },
          "content": { "type": "string" },
          "timestamp": { "type": "string", "format": "date-time" }
        }
      }
    },
    "summary": { "type": "string" }
  }
}

4. Skill Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "SkillObject",
  "type": "object",
  "required": ["skill_id", "name", "version", "wasm_hash", "capability_schema"],
  "properties": {
    "skill_id": { "type": "string", "format": "uuid" },
    "name": { "type": "string" },
    "version": { "type": "string" },
    "wasm_hash": { "type": "string" },
    "capability_schema": {
      "type": "object",
      "required": ["methods", "permissions"],
      "properties": {
        "methods": {
          "type": "array",
          "items": { "type": "string" }
        },
        "permissions": {
          "type": "array",
          "items": { "type": "string" }
        }
      }
    }
  }
}

5. Task Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "TaskObject",
  "type": "object",
  "required": ["task_id", "parent_goal_id", "priority", "state", "context", "execution"],
  "properties": {
    "task_id": { "type": "string", "format": "uuid" },
    "parent_goal_id": { "type": "string", "format": "uuid" },
    "priority": { "type": "integer", "minimum": 0, "maximum": 7 },
    "state": { "type": "string", "enum": ["FORMULATED", "READY", "RUNNING", "WAITING", "SUSPENDED", "FULFILLED", "ABORTED"] },
    "context": {
      "type": "object",
      "required": ["variables", "active_entities"],
      "properties": {
        "variables": {
          "type": "object",
          "additionalProperties": { "type": "string" }
        },
        "active_entities": {
          "type": "array",
          "items": { "type": "string", "format": "uuid" }
        }
      }
    },
    "execution": {
      "type": "object",
      "required": ["worker_id", "concurrency_slot", "current_step_index"],
      "properties": {
        "worker_id": { "type": "string", "format": "uuid" },
        "concurrency_slot": { "type": "integer", "minimum": 0, "maximum": 7 },
        "current_step_index": { "type": "integer", "minimum": 0 }
      }
    }
  }
}

6. Observation Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ObservationObject",
  "type": "object",
  "required": ["observation_id", "source_type", "raw_content", "timestamp"],
  "properties": {
    "observation_id": { "type": "string", "format": "uuid" },
    "source_type": { "type": "string", "enum": ["AUDIO", "SCREEN_CAPTURE", "USER_INPUT", "NOTIFICATION"] },
    "raw_content": { "type": "string" },
    "timestamp": { "type": "string", "format": "date-time" }
  }
}

7. Reflection Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ReflectionObject",
  "type": "object",
  "required": ["reflection_id", "target_task_id", "analysis", "lesson_learned", "valence"],
  "properties": {
    "reflection_id": { "type": "string", "format": "uuid" },
    "target_task_id": { "type": "string", "format": "uuid" },
    "analysis": { "type": "string" },
    "lesson_learned": { "type": "string" },
    "valence": { "type": "string", "enum": ["POSITIVE", "NEUTRAL", "NEGATIVE"] }
  }
}

8. Plan Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "PlanObject",
  "type": "object",
  "required": ["plan_id", "parent_goal_id", "steps", "estimated_resource_budgets"],
  "properties": {
    "plan_id": { "type": "string", "format": "uuid" },
    "parent_goal_id": { "type": "string", "format": "uuid" },
    "steps": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["step_id", "tool_name", "parameters"],
        "properties": {
          "step_id": { "type": "string", "format": "uuid" },
          "tool_name": { "type": "string" },
          "parameters": { "type": "object" }
        }
      }
    },
    "estimated_resource_budgets": {
      "type": "object",
      "required": ["token_budget", "api_cost_usd"],
      "properties": {
        "token_budget": { "type": "integer", "minimum": 0 },
        "api_cost_usd": { "type": "number", "minimum": 0.0 }
      }
    }
  }
}

9. Execution Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ExecutionObject",
  "type": "object",
  "required": ["execution_id", "task_id", "allocated_slot", "exit_code", "result_payload"],
  "properties": {
    "execution_id": { "type": "string", "format": "uuid" },
    "task_id": { "type": "string", "format": "uuid" },
    "allocated_slot": { "type": "integer", "minimum": 0, "maximum": 7 },
    "exit_code": { "type": "integer" },
    "result_payload": { "type": "object" }
  }
}

10. Thought Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ThoughtObject",
  "type": "object",
  "required": ["thought_id", "parent_task_id", "proposer_assertion", "critic_challenge", "synthesis_resolution"],
  "properties": {
    "thought_id": { "type": "string", "format": "uuid" },
    "parent_task_id": { "type": "string", "format": "uuid" },
    "proposer_assertion": { "type": "string" },
    "critic_challenge": { "type": "string" },
    "synthesis_resolution": { "type": "string" }
  }
}

11. Event Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "EventObject",
  "type": "object",
  "required": ["event_id", "correlation_id", "event_type", "payload"],
  "properties": {
    "event_id": { "type": "string", "format": "uuid" },
    "correlation_id": { "type": "string", "format": "uuid" },
    "event_type": { "type": "string" },
    "payload": { "type": "object" }
  }
}

12. Tool Result Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ToolResultObject",
  "type": "object",
  "required": ["result_id", "step_id", "tool_name", "status", "data_payload"],
  "properties": {
    "result_id": { "type": "string", "format": "uuid" },
    "step_id": { "type": "string", "format": "uuid" },
    "tool_name": { "type": "string" },
    "status": { "type": "string", "enum": ["SUCCESS", "FAILED", "TIMEOUT"] },
    "data_payload": { "type": "object" }
  }
}

13. Knowledge Node Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "KnowledgeNodeObject",
  "type": "object",
  "required": ["node_id", "label", "properties", "embedding_vector"],
  "properties": {
    "node_id": { "type": "string", "format": "uuid" },
    "label": { "type": "string" },
    "properties": { "type": "object" },
    "embedding_vector": {
      "type": "array",
      "items": { "type": "number" }
    }
  }
}

14. Relationship Edge Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "RelationshipEdgeObject",
  "type": "object",
  "required": ["edge_id", "source_node_id", "target_node_id", "relation_type", "strength"],
  "properties": {
    "edge_id": { "type": "string", "format": "uuid" },
    "source_node_id": { "type": "string", "format": "uuid" },
    "target_node_id": { "type": "string", "format": "uuid" },
    "relation_type": { "type": "string" },
    "strength": { "type": "number", "minimum": 0.0, "maximum": 1.0 }
  }
}

15. Memory Chunk Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "MemoryChunkObject",
  "type": "object",
  "required": ["chunk_id", "raw_text", "metadata", "embedding_vector"],
  "properties": {
    "chunk_id": { "type": "string", "format": "uuid" },
    "raw_text": { "type": "string" },
    "metadata": { "type": "object" },
    "embedding_vector": {
      "type": "array",
      "items": { "type": "number" }
    }
  }
}

16. Embedding Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "EmbeddingObject",
  "type": "object",
  "required": ["vector_id", "dimensions", "values", "model_signature"],
  "properties": {
    "vector_id": { "type": "string", "format": "uuid" },
    "dimensions": { "type": "integer", "minimum": 1 },
    "values": {
      "type": "array",
      "items": { "type": "number" }
    },
    "model_signature": { "type": "string" }
  }
}

17. Checkpoint Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CheckpointObject",
  "type": "object",
  "required": ["checkpoint_id", "version", "timestamp", "diff_payload", "checksum"],
  "properties": {
    "checkpoint_id": { "type": "string", "format": "uuid" },
    "version": { "type": "integer", "minimum": 1 },
    "timestamp": { "type": "string", "format": "date-time" },
    "diff_payload": { "type": "object" },
    "checksum": { "type": "string" }
  }
}

18. Snapshot Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "SnapshotObject",
  "type": "object",
  "required": ["snapshot_id", "timestamp", "full_state_payload", "checksum"],
  "properties": {
    "snapshot_id": { "type": "string", "format": "uuid" },
    "timestamp": { "type": "string", "format": "date-time" },
    "full_state_payload": { "type": "object" },
    "checksum": { "type": "string" }
  }
}

19. Journal Entry Object Schema Specification (JSON-Schema)

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "JournalEntryObject",
  "type": "object",
  "required": ["entry_id", "timestamp", "summary", "valence_marker"],
  "properties": {
    "entry_id": { "type": "string", "format": "uuid" },
    "timestamp": { "type": "string", "format": "date-time" },
    "summary": { "type": "string" },
    "valence_marker": { "type": "string", "enum": ["POSITIVE", "NEUTRAL", "NEGATIVE"] }
  }
}

Part VI — Memory Graph

CASPER utilizes a multi-layered, semantic memory graph to represent entity
relationships, goals, dependencies, and execution logs, coordinating
associations across distinct conceptual domains.

+--------------------------------------------------------------------------------------------------+
|                                    SEMANTIC GRAPH STRUCTURE                                      |
+--------------------------------------------------------------------------------------------------+
|                                                                                                  |
|     +------------------+             +------------------+             +------------------+       |
|     |  Goal Lattice    |             |  Knowledge Graph |             | Entity Graph     |       |
|     |  - Sub-Goals     |             |  - Concepts      |             | - People         |       |
|     |  - Milestones    |             |  - Shared Rules  |             | - Workspace Files|       |
|     +------------------+             +------------------+             +------------------+       |
|              |                                |                                |                 |
|              +--------------------------------+--------------------------------+                 |
|                                               |                                                  |
|                                               v                                                  |
|                                    +----------------------+                                      |
|                                    | Relational Edge Maps |                                      |
|                                    | - Temporal Index     |                                      |
|                                    | - Somatic Valence    |                                      |
|                                    +----------------------+                                      |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+

6.1 Multi-Layered Graph Domains

  - Knowledge Graph: Maps abstract concepts, systemic facts, and operational
    rules (e.g., "Keto dietary restrictions require carb limits").
  - Goal Graph: Tracks user objectives, active milestones, and task
    dependencies, ensuring all tasks support defined parent goals.
  - Entity Graph: Represents real-world objects, contacts, documents, and system
    files, tracking relationships and usage patterns.
  - Conversation Graph: Organizes dialog histories and sentiment trends across
    chat sessions.
  - Execution Graph: Chronological map of worker runs, task outcomes, and
    process execution flows.

6.2 Graph Traversals and Optimization Loops

flowchart TD
    A[Trigger Context Query] --> B[Retrieve active entity nodes]
    B --> C[Run Cypher-like semantic path traversal]
    
    C --> D[Identify associated milestones & goals]
    D --> E[Filter paths via temporal decay & valence scores]
    E --> F[Select high-value relational context nodes]
    
    F --> G[Optimize Graph: Merge duplicate entities]
    G --> H[Self-Repair: Rebuild disconnected graph edges]

  - Somatic Marker Weighting: Edges are weighted using a somatic valence index.
    Success or failure records adjust edge values dynamically, prioritizing
    high-value paths in future planning:

\mathbf{W}_{edge} = \mathbf{W}_{base} \times (1 + \mathbf{V}_{valence} \cdot \mathbf{R}_{recency})

  - Self-Repairing Loop: During consolidation periods, the Graph Engine scans
    for orphan nodes or broken dependency links, running path repair routines
    and merging duplicate entities to keep database index traversals optimized.

Part VII — Memory Indexing

CASPER employs multiple indexed data models to support rapid keyword searches,
relational graph traversals, and high-dimensional semantic vector queries.

flowchart TD
    A[Incoming Memory Query] --> B{Select Target Index}
    
    B -->|Fast Variable / Hash Lookup| C[In-Memory Hash Index]
    B -->|Directory / Path Matching| D[Prefix Trie Index]
    B -->|Structured Transaction Logs| E[B-Tree Index SQLite]
    B -->|High-Dimensional Semantic Search| F[Hierarchical HNSW Vector Index]

7.1 Multi-Tier Index Selection and Runtime Usage

| Index Model      | Backend Engine  | Target Use Case                    | Latency            | Eviction Policy       | Justification                                         |
| :--------------- | :-------------- | :--------------------------------- | :----------------- | :-------------------- | :---------------------------------------------------- |
| **Hash Index**   | In-Memory Cache | Active task variables, TSB lookups | $< 0.1\text{ms}$   | Least Recently Used   | Fast, constant-time key-value retrieval.              |
| **Trie Index**   | Prefix Registry | Directory searches, path matching  | $< 0.5\text{ms}$   | On-demand unload      | Optimized prefix routing for file and system paths.   |
| **B-Tree Index** | SQLite Database | Transaction records, chronicles    | $\leq 2\text{ms}$  | Manual vacuuming      | Balanced search and insertion for structured logs.    |
| **HNSW Index**   | Qdrant DB       | High-dimensional vector searches   | $\leq 15\text{ms}$ | Graph compaction      | Fast approximate nearest neighbor search.             |
| **IVF Index**    | Qdrant DB       | Large-scale candidate pruning      | $\leq 10\text{ms}$ | Sweep compaction      | Inverted file index for filtering large memory pools. |
| **ScaNN Index**  | Qdrant DB       | Hardware-accelerated searches      | $\leq 8\text{ms}$  | Direct memory mapping | Vector quantization utilizing CPU AVX instructions.   |

Part VIII — Retrieval Engine

The Retrieval Engine compiles and ranks associative memory nodes, constructing
clear, contextual prompt payloads to guide active reasoning sessions.

flowchart TD
    A[Incoming User Command] --> B[Intent Analysis & Entity Extraction]
    B --> C[Generate Multi-Tier Memory Query Plan]
    
    C --> D[Run Hybrid Retrieval: Vector + Keyword + Graph Search]
    D --> E[Compile Memory Candidates & Metadata]
    
    E --> F[Ranking Pipeline: Cosine, Recency, Importance, Valence]
    F --> G[Context Building: De-duplicate & Sort Chunks]
    
    G --> H[Prompt Assembly: Pack Context Window Schema]
    H --> I[Dispatch Context Payload to Reasoning Engine]

8.1 Candidate Scoring Optimization Algorithm

The Retrieval Engine evaluates memory candidate relevance using a
multi-dimensional ranking algorithm, prioritizing nodes that support active
goals and possess strong valence markers:

\text{Score}(m, c) = w_{sim} \cdot \mathbf{S}_{sim}(m, c) + w_{time} \cdot e^{-\lambda \Delta t} + w_{val} \cdot \mathbf{V}(m) + w_{goal} \cdot \mathbf{G}(m)

Where:

  - \mathbf{S}_{sim}(m, c) is the cosine semantic similarity between memory node
    m and query context c.
  - e^{-\lambda \Delta t} represents temporal decay, adjusting priority based on
    elapsed time \Delta t.
  - \mathbf{V}(m) is the somatic valence marker value of the candidate.
  - \mathbf{G}(m) represents active goal relevance, weighting nodes linked to
    current milestones.
  - w_{sim}, w_{time}, w_{val}, w_{goal} are prioritizing weights balanced
    dynamically by the system.

8.2 Retrieval Engine Execution Specification

procedure ExecuteRetrievalPipeline(query: string, active_goal_id: UUID)
    // 1. Analyze intent and extract query entities
    entities = extract_query_entities(query)
    query_vector = generate_query_embedding(query)
    
    // 2. Query separate memory tiers
    vector_candidates = search_vector_index(query_vector, limit=50)
    graph_candidates = traverse_knowledge_graph(entities, active_goal_id)
    chronicle_candidates = query_transaction_logs(entities)
    
    // 3. Aggregate and de-duplicate memory candidates
    all_candidates = merge_candidates(vector_candidates, graph_candidates, chronicle_candidates)
    
    // 4. Rank candidates using scoring rules
    for each candidate in all_candidates do
        candidate.score = calculate_retrieval_score(candidate, query_vector, active_goal_id)
    end
    
    sort_candidates_by_score(all_candidates)
    
    // 5. Pack context windows, respecting token boundaries
    context_window = initialize_context_window(max_tokens=16000)
    for each candidate in all_candidates do
        if has_available_token_budget(context_window, candidate) then
            pack_chunk_into_context(context_window, candidate)
        else
            break
        end
    end
    
    return context_window
end procedure

Part IX — Memory Consolidation

+--------------------------------------------------------------------------------------------------+
|                               MEMORY CONSOLIDATION PIPELINE                                      |
+--------------------------------------------------------------------------------------------------+
|                                                                                                  |
|  [ Working Memory L1 ] ------------------------------> [ Short-Term Memory SQLite ]              |
|  - Active task variables & parameters                   - Event logs & chronological journals    |
|                                                                                                  |
|                                                                                                  |
|  [ Semantic Graph L4 ] <----------------------------- [ Nightly Replay & Dreaming Simulation ]   |
|  - Refined entity-relation models                       - DBSCAN Clustering & Summarization      |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+

CASPER's Memory Consolidation pipeline runs as a daily background maintenance
cycle during low-activity periods. It reviews episodic logs, compresses raw run
details into summary chronicles, and integrates new nodes and edges into the
semantic knowledge graph.

9.1 Consolidation and Replay Loop

flowchart TD
    A[Begin Nightly Maintenance Cycle] --> B[Retrieve raw episodic logs from past 24 hours]
    B --> C[Run DBSCAN clustering over log embeddings]
    
    C --> D[Identify dense experience clusters]
    D --> E[Summarize clusters & generate long-term chronicles]
    
    E --> F[Extract entities, relations, and somatic markers]
    F --> G[Neo4j: Merge entities and update edge weights]
    G --> H[Qdrant: Insert consolidated vector payloads]
    
    H --> I[Prune temporary logs, vacuum database index files]

  - Dreaming Simulation: During consolidation, the system runs offline
    simulations to evaluate past task failures. It tests alternative path
    options and records successful recovery strategies to procedural memory.
  - Semantic Integration: Discovered concepts and habits are integrated with
    existing long-term beliefs, pruning outdated preferences and resolving
    logical conflicts.

Part X — Forgetting Engine

The Forgetting Engine balances storage footprints and retrieval performance,
pruning low-value or redundant data while protecting critical system
configurations.

flowchart TD
    A[Initiate Forgetting Scan] --> B[Calculate Candidate Decay & Value Metrics]
    B --> C{Evaluate System Safety & Trust Rules}
    
    C -->|Core Identity Rules| D[Locked Node: Never evict]
    C -->|Low Value / High Redundancy| E[Eviction: Archive payload]
    
    E --> F[Encrypt and compress raw chronicle files]
    F --> G[Prune indexes and clear database storage space]

10.1 Eviction Policy Matrix and Rules

  - Least Recently Used (LRU): Applied to fast-path in-memory caches. Discards
    the oldest accessed chunks to free up active memory pages.
  - Adaptive Replacement Cache (ARC): Automatically balances between frequency
    and recency, optimizing memory allocations for dynamic workloads.
  - Temporal Value Decay: Long-term episodic records naturally decay in priority
    over time, calculated using an exponential decay model:

\mathbf{W}_{decay}(t) = \mathbf{W}_{base} \cdot e^{-\lambda \cdot (t - t_{last})}

Where \lambda represents the decay coefficient and (t - t_{last}) is the elapsed
time since the node was last accessed.

  - Semantic Redundancy Pruning: Identifies highly similar duplicate logs,
    keeping consolidated summary points while deleting individual raw trace
    records.

Part XI — Context Engine

The Context Engine coordinates active focus windows, sorting retrieved memory
nodes to construct clear, goal-directed prompts for reasoning sessions.

flowchart TD
    A[Extract Active Task TSB] --> B[Retrieve High-Scoring Memory Candidates]
    B --> C[Validate token budget boundaries]
    
    C --> D[Sort and prioritize context chunks]
    D --> E[Chunk Packing: Order by goal and recency]
    E --> F[Compress and format prompt variables]
    
    F --> G[Dispatch complete Context Window schema]

11.1 Context Window Composition and Packing Rules

To provide clear context boundaries for reasoning engines, the Context Engine
formats and structures the active focus window:

  - Token Budget Allocation: Budgets are divided across key task zones: Active
    Goal and Tasks (30%), Conversation History (25%), Relational Memory Context
    (35%), and Telemetry Logs (10%).
  - Chunk Ordering: Retrieved memories are ordered chronologically, ensuring
    that recent events and active goal milestones are positioned first.
  - Context Repair: If a task thread encounters an error, the Context Engine
    rebuilds the focus window, pulling in similar past failures to assist with
    recovery planning.

Part XII — Embedding Pipeline

CASPER's Embedding Pipeline processes raw text, images, and system events,
generating normalized, high-dimensional vector representations for semantic
search.

flowchart TD
    A[Incoming Raw Text / Event Document] --> B[Document Chunking & Parsing]
    B --> C[Generate Metadata Attributes JSON]
    
    C --> D[Queue Batch Payload for GPU Acceleration]
    D --> E[Generate Vector Embeddings using Native Models]
    
    E --> F[Normalize vector values]
    F --> G[Insert payload & indexes into Qdrant Vector DB]

12.1 Segmenting and Vector Quantization Specs

  - Dynamic Chunking: Documents are segmented using semantic boundaries (e.g.,
    paragraph transitions, system event blocks) rather than fixed token lengths,
    preserving structural context.
  - Quantization & Normalization: Vector dimensions are normalized using L2
    normalization and quantized to reduce memory requirements while maintaining
    search accuracy.
  - In-Memory Caching: High-frequency vector results are cached locally,
    bypassing embedding model execution for repeating queries.

Part XIII — Reflection Engine

The Reflection Engine evaluates completed and aborted tasks, updating procedural
templates, updating belief rules, and correcting memory indexes based on task
results.

flowchart TD
    A[Task Completion / Failure Alert] --> B[Retrieve Task Execution Chronicle Logs]
    B --> C[Self-Appraisal: Compare actual outcomes to predictions]
    
    C --> D{Did execution result fail?}
    D -->|Yes| E[Perform Error Analysis & write lesson learned]
    D -->|No| F[Perform Success Analysis & optimize skill template]
    
    E & F --> G[Belief Revision: Update entity & preference records]
    G --> H[Somatic Marker Update: Adjust edge weights in Neo4j]

13.1 Meta-Cognitive Appraisal & Belief Refinement

  - Self-Appraisal Loops: The engine reviews completed task logs to identify
    planning inefficiencies or errors, updating procedural templates to prevent
    repeating mistakes.
  - Somatic Marker Updates: Adjusts edge strengths in the semantic knowledge
    graph based on performance, prioritizing successful execution paths in
    future runs.
  - Belief Refinement: Resolves contradictions between new observations and
    existing long-term knowledge, ensuring consistent beliefs across the system.

Part XIV — World Model

The World Model coordinates spatial, temporal, entity, and capability
representations, providing CASPER with a structured view of its workspace.

+--------------------------------------------------------------------------------------------------+
|                                    WORLD MODEL TOPOLOGY                                          |
+--------------------------------------------------------------------------------------------------+
|                                                                                                  |
|  [ Entity Layer ] -------------------------------------------------> [ Relationship Layer ]       |
|  - Contacts, files, tools, calendars                                 - Trust levels, usage paths |
|                                                                                                  |
|                                                                                                  |
|  [ Capability Layer ] <--------------------------------------------> [ Spatial & Temporal Layer ] |
|  - Registered skills, APIs, methods                                  - Physical environments     |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+

  - Spatial Mapping: Indexes physical coordinates and workspace directory paths,
    allowing the system to locate files and folders efficiently.
  - Temporal Timelines: Keeps chronological event logs of system tasks, user
    notifications, and calendar milestones.
  - Capability Index: Indexes registered skills, system tools, and APIs,
    providing the system with a clear directory of available capabilities.

Part XV — Checkpoint System

The Checkpoint System manages full and incremental state backups, allowing the
system to restore operations safely after crashes or errors.

stateDiagram-v2
    [*] --> Tracking : Active Task Executing
    
    Tracking --> SaveCheckpoint : Scheduled Timer Event
    state SaveCheckpoint {
        [*] --> GenerateDiff
        GenerateDiff --> WriteIncrementalDiff : Commit State Delta
        WriteIncrementalDiff --> VerifyChecksum
    }
    
    SaveCheckpoint --> Tracking : Verification Success
    SaveCheckpoint --> RollbackState : Verification Fail / Write Error
    
    RollbackState --> Tracking : Restore Last Verified Snapshot

  - Incremental Snapshots: Saves step delta differences to disk during task
    runs, minimizing storage overhead while providing clear restore points.
  - Full Backups: Generates complete system snapshots during consolidation
    periods, providing secure baseline recovery packages.
  - Integrity Validation: Uses cryptographic checksums to verify backups before
    restoration, protecting recovery pipelines from corrupt or manipulated data.

Part XVI — Synchronization

CASPER supports secure cross-device state synchronization, utilizing
Conflict-Free Replicated Data Types (CRDTs) to reconcile modifications across
user devices.

sequenceDiagram
    autonumber
    participant Local as Local Host Client
    participant Sync as Sync Router Engine
    participant Cloud as Secure Cloud Replica
    participant Peer as Peer Mesh Client

    Local->>Local: Perform local memory updates (CRDT delta)
    Local->>Sync: Dispatch Sync Event (Version Vector)
    
    activate Sync
    Sync->>Peer: Stream encrypted change deltas over P2P mesh
    Sync->>Cloud: Commit change deltas to cloud replica
    
    activate Cloud
    Cloud->>Cloud: Resolve conflicts, merge state histories
    Cloud-->>Sync: Return Sync Confirmation
    deactivate Cloud
    
    Sync-->>Local: Replicas aligned successfully
    deactivate Sync

  - CRDT Reconciliations: Employs LWW-Element-Set (Last-Write-Wins-Element-Set)
    models to merge conflicting updates from separate devices without requiring
    a central coordinator.
  - Offline-First Storage: Stores state changes locally when disconnected,
    queueing updates to synchronize immediately upon reconnecting to the
    network.
  - Peer-to-Peer Sync: Synchronizes data directly across local user devices over
    peer-to-peer networks, bypassing cloud transfers when possible.

Part XVII — Memory Security

The Security Manager enforces strict, zero-trust data protection policies,
encrypting all memory payloads and verifying access paths before data is
exposed.

flowchart TD
    A[Memory Read / Write Request] --> B[Verify Caller Security Tokens]
    B --> C{Are credentials verified?}
    
    C -->|Yes| D[Decrypt Target Payload (AES-256-GCM)]
    C -->|No| E[Trigger Security Alert & Deny Access]
    
    D --> F[Run Privacy Filter: Redact sensitive identifiers]
    F --> G[Deliver Sanitized Data to Caller]

  - Zero-Knowledge Sync: Encrypts state and memory databases locally using keys
    managed by hardware-backed systems (e.g., macOS Keychain) before
    synchronizing to cloud systems.
  - Hardware Cryptography: Offloads key generation and encryption operations to
    host hardware security modules (TPM/Secure Enclave).
  - Sanitization Interceptors: Scrapes outbound data flows, identifying and
    redacting sensitive data (such as credit cards or passwords) unless
    explicitly authorized.

Part XVIII — Performance Engineering

CASPER is engineered with a lightweight system footprint, using optimized
indices and memory compaction to maintain fast access times on standard
hardware:

| Metric                      | Target (Optimal)      | Hard Limit           | Measuring Protocol                               | Systems Impact                                      |
| :-------------------------- | :-------------------- | :------------------- | :----------------------------------------------- | :-------------------------------------------------- |
| **Working Memory Latency**  | $\leq 1\text{ms}$     | $5\text{ms}$         | Measured during RAM key-value lookups.           | Direct impact on active execution speeds.           |
| **Episodic Query Latency**  | $\leq 30\text{ms}$    | $100\text{ms}$       | Measured during SQLite B-Tree database queries.  | Determines retrieval and planning response times.   |
| **Vector Search Latency**   | $\leq 15\text{ms}$    | $50\text{ms}$        | Approximate Nearest Neighbor lookup in Qdrant.   | Directly affects semantic context retrieval speeds. |
| **Graph Traversal Latency** | $\leq 20\text{ms}$    | $80\text{ms}$        | Path traversal checks in Neo4j databases.        | Determines entity-relationship retrieval times.     |
| **Consolidation Duration**  | $\leq 15\text{ mins}$ | $45\text{ mins}$     | Runs during offline nightly maintenance periods. | Keeps databases index-optimized with minimal wear.  |
| **Embedding Throughput**    | $50\text{ docs/sec}$  | $10\text{ docs/sec}$ | Batch document processing using native models.   | Influences directory indexing performance.          |

Part XIX — Failure Recovery

The Memory Watchdog daemon monitors database health, automatically running
repair and restore routines if data corruption or access failures are detected.

flowchart TD
    A[Database Read Failure / Corruption Detected] --> B[Halt target memory subsystem]
    B --> C[Run Integrity Check: Identify corrupted indexes]
    
    C --> D{Is corruption repairable?}
    D -->|Yes| E[Run SQLite Repair & Rebuild Vector Indexes]
    D -->|No| F[Retrieve last verified snapshot from backup log]
    
    E & F --> G[Replay transaction journal logs to current state]
    G --> H[Verify database checksums & restart memory subsystem]

  - Journal Replays: Uses write-ahead transaction logs to replay lost state
    changes after database crashes or unexpected shutdowns.
  - Index Regeneration: Rebuilds corrupted trie, HNSW, or B-tree indexes
    automatically without requiring complete database restores.
  - Corrupted Chunk Recovery: If an episodic or vector chunk is corrupted, the
    system uses source document logs to re-segment and regenerate the missing
    vectors.

Part XX — Observability

The monitoring engine collects detailed telemetry logs across all memory
systems, providing performance metrics and queue warnings on the host user
interface.

+-------------------------------------------------------------------------+
|                        MEMORY TELEMETRY EVENT                           |
+-------------------------------------------------------------------------+
|                                                                         |
|  [ DATABASE STATUS ]                                                    |
|  SQLite DB Connections: 4 Active                                        |
|  Qdrant Vectors Registered: 120,401 Points                              |
|  Neo4j Entities: 4,012 Nodes | Edges: 24,192 Relations                  |
|                                                                         |
|  [ CACHE EFFICIENCY ]                                                   |
|  Working Memory Cache Hits: 98.4%                                       |
|  Vector Cache Hits: 84.1%                                               |
|                                                                         |
|  [ STORAGE PERFORMANCE ]                                                |
|  Database Fragmentation Index: 2.1%                                     |
|  Average Retrieval Latency: 12.4ms                                      |
|                                                                         |
+-------------------------------------------------------------------------+

Part XXI — Configuration

CASPER uses structured YAML configurations to define resource allocations,
storage directories, and indexing rules for all memory systems.

version: "2.0.0"
component: "memory_manager"

storage:
  directories:
    data_root: "/var/lib/casper/"
    working_memory_db: "sqlite:///var/lib/casper/working.db"
    episodic_db: "sqlite:///var/lib/casper/episodic.db"
    vector_store_url: "http://localhost:6333"
    graph_db_url: "bolt://localhost:7687"
  checkpoint_interval_ms: 60000

allocation:
  ram_budget_mb:
    working_memory_pool: 256
    active_context_pool: 512
    temporary_cache_pool: 128
  eviction_threshold_pressure: 0.85

indexing:
  vector_dimension: 1536
  metric: "cosine"
  hnsw_m: 16
  ef_construct: 64
  full_scan_threshold: 1000

consolidation:
  schedule_cron: "0 2 * * *"
  dbscan_eps: 0.5
  dbscan_min_samples: 5
  compression_ratio_target: 0.25

security:
  encryption_algorithm: "AES-256-GCM"
  key_management: "macos_keychain"
  redact_pii: true

Part XXII — APIs

The Memory System exposes structured gRPC interfaces to support rapid,
programmatic database searches, transactional updates, and synchronization.

syntax = "proto3";
package casper.memory.v2;

service MemoryService {
  rpc QueryMemory (MemoryQueryRequest) returns (MemoryQueryResponse);
  rpc UpdateMemory (MemoryUpdateRequest) returns (MemoryUpdateResponse);
  rpc CreateCheckpoint (CheckpointRequest) returns (CheckpointResponse);
  rpc RestoreState (RestoreStateRequest) returns (RestoreStateResponse);
  rpc StreamSyncUpdates (stream SyncPayload) returns (stream SyncAcknowledgement);
}

message MemoryQueryRequest {
  string query_text = 1;
  string target_goal_id = 2;
  repeated string entity_filters = 3;
  int32 max_results = 4;
}

message MemoryQueryResponse {
  repeated MemoryChunk chunks = 1;
  double search_duration_ms = 2;
}

message MemoryChunk {
  string chunk_id = 1;
  string text_content = 2;
  string metadata_json = 3;
  double score = 4;
}

message MemoryUpdateRequest {
  string transaction_id = 1;
  repeated MemoryUpdatePayload updates = 2;
}

message MemoryUpdatePayload {
  string object_type = 1;
  string action = 2; // INSERT, UPDATE, DELETE
  string payload_json = 3;
}

message MemoryUpdateResponse {
  bool is_success = 1;
  string error_message = 2;
}

message CheckpointRequest {
  string parent_task_id = 1;
}

message CheckpointResponse {
  string checkpoint_id = 1;
  string checksum = 2;
}

message RestoreStateRequest {
  string checkpoint_id = 1;
}

message RestoreStateResponse {
  bool is_success = 1;
}

message SyncPayload {
  string device_id = 1;
  int64 vector_version = 2;
  string encrypted_delta = 3;
}

message SyncAcknowledgement {
  string ack_id = 1;
  bool is_aligned = 2;
}

Part XXIII — Testing

To verify system reliability and safety, CASPER runs automated integration,
stress, and chaos testing pipelines across memory systems:

flowchart TD
    A[Initialize Memory Testing Pipeline] --> B[Run Unit & Integration Tests]
    B --> C[Trigger Stress & Chaos Tests]
    
    subgraph Testing Suite
    D[Fault Injector: Corrupt SQLite database pointers]
    E[Resource Limiter: Force low-memory alerts]
    F[Sync Scrambler: Inject network merge conflicts]
    end
    
    C --> D & E & F
    
    D & E & F --> G[Verify recovery paths and check for data loss]
    G --> H{Do verification checks pass?}
    
    H -->|Yes| I[Approve memory system deployment]
    H -->|No| J[Reject, Log Exception, Notify Developers]

  - Fault Injection Tests: Corrupts SQLite database tables during active task
    writes to verify automatic journal replays and checksum-based snapshot
    restorations.
  - Memory Pressure Stressing: Forces high-volume database query tasks under
    tight RAM limits, verifying that virtual cognitive paging and cache
    evictions clear memory pages correctly.
  - Sync Conflict Scrambling: Injects out-of-order state modifications across
    multiple peer clients, verifying that the CRDT engine resolves merge
    conflicts consistently.

Part XXIV — Future Evolution

timeline
    title Memory System Milestones
    Phase 1 : Static Structured Databases : Multi-database storage : B-tree & HNSW indexing
    Phase 2 : Continual Learning Networks : Autonomous dynamic vector optimizations : Continuous background consolidations
    Phase 3 : Distributed Neural Memory Mesh : Decentralized cross-device networks : Fully integrated personal knowledge fabric

  - Continual Learning Optimization: Future memory engines will automatically
    adjust vector dimensions and quantization parameters in response to active
    task workloads, optimizing lookup accuracy.
  - Predictive Pre-Caching: Memory systems will analyze daily user habits to
    pre-load relevant context, documents, and tool settings before tasks start,
    minimizing startup latencies.
  - Decentralized Cognitive fabrics: Transitions memory systems into fully
    distributed meshes, securely synchronizing personal experience and knowledge
    databases across devices without relying on central servers.
