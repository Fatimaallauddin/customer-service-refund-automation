# Customer Service Automation — Technical Workflow Overview

This document summarizes the architecture and technical components of an n8n customer-service automation prototype. The implementation was developed and tested as a prototype, not deployed as a production customer-service system.

## Workflow structure

The primary n8n workflow contains **52 nodes**, distributed as follows:

| n8n node type | Count |
|---|---:|
| Code | 10 |
| Data Table | 9 |
| HTTP Request | 4 |
| IF | 6 |
| No Operation | 2 |
| Set | 1 |
| Switch | 6 |
| Telegram | 13 |
| Telegram Trigger | 1 |
| **Total** | **52** |

The project also contains **16 separate development and testing workflows** used to explore and verify individual components. These are supporting development artifacts, not independently deployed services.

## Main workflow components

### 1. Telegram messaging
- Receives customer-service requests through a Telegram trigger.
- Sends prompts, validation feedback, status messages and results.

### 2. Conversation and session management
- Uses n8n Data Tables to manage chat-specific session records.
- Routes incoming messages according to commands and conversation state.
- Supports session initialization, reset and state updates.

### 3. Booking validation
- Extracts and validates booking identifiers.
- Looks up booking records in a test dataset.
- Routes invalid or missing bookings to appropriate response branches.

### 4. Rail information retrieval
- Uses HTTP Request nodes to retrieve rail timetable information.
- Applies JavaScript logic for station resolution, timetable matching and service-status interpretation.
- Includes conditional paths for incomplete or unmatched rail information.

### 5. Deterministic refund decisions
- Applies predefined test rules through JavaScript Code nodes.
- Separates booking and rail-data validation from refund eligibility decisions.
- Produces a result message without executing a financial transaction.

### 6. Error handling and safe responses
- Includes branches for unsupported messages, invalid booking identifiers and insufficient data.
- Provides safe responses when the workflow cannot establish a reliable outcome.

## Implementation status

| Category | Scope |
|---|---|
| **Implemented** | Telegram messaging, session management, test booking lookup, rail API lookup stages, JavaScript validation and matching, deterministic refund logic, and conditional response branches. |
| **Offline-tested separately** | AI-based intent classification; not integrated into the primary workflow. |
| **Not implemented or demonstrated for production** | Live customer booking integration, actual refund execution, production-scale load testing, enterprise monitoring and production human escalation. |

## Technical boundaries

- The workflow was not deployed to real customers.
- Booking records were test data rather than live customer records.
- No real refunds were processed.
- The refund rules are prototype/test logic, not a transport operator's official policy.
- The public repository does not include runnable workflow exports, API credentials, source JavaScript or live datasets.

For the high-level process diagram, see the repository's `README.md`. The sanitized, non-executable node-and-edge representation is available in `architecture/workflow-topology.json`.
