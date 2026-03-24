# Hatchet Span Conventions

## Overview

This canvas defines the vocabulary of operations (spans) that Hatchet services emit. All span names follow the `hatchet.{operation}` pattern.

## Span Kinds

| Kind | Purpose | Examples |
|------|---------|----------|
| CONSUMER | Worker handling tasks from queue | `start_step_run`, `cancel_step_run` |
| PRODUCER | Triggering operations | `run_workflow`, `push_event` |
| INTERNAL | Internal operations | `durable.wait_for` |

## Span Patterns

### Consumer Spans (Worker)

| Pattern | Description |
|---------|-------------|
| `hatchet.start_step_run` | Main span for step function execution |
| `hatchet.cancel_step_run` | Handles step cancellation requests |

### Producer Spans (Triggers)

| Pattern | Description |
|---------|-------------|
| `hatchet.run_workflow` | Triggers a single workflow |
| `hatchet.run_workflows` | Bulk triggers multiple workflows |
| `hatchet.push_event` | Publishes a single event |
| `hatchet.bulk_push_event` | Publishes multiple events |
| `hatchet.schedule_workflow` | Creates scheduled workflow execution |

### Internal Spans (SDK)

| Pattern | Description |
|---------|-------------|
| `hatchet.durable.wait_for` | Durable context wait operations |

## Server-Side Spans (hatchet-engine)

Server-side spans use the `hatchet.run/*` namespace and are emitted by engine components.

| Pattern | Component | Description |
|---------|-----------|-------------|
| HTTP Request spans | API Server | HTTP request handling |
| `hatchet.run/ingest-event` | Ingestor | Event ingestion processing |
| `hatchet.run/task-controller` | Task Controller | Task state transitions |
| `hatchet.run/handle-check-queue` | Scheduler | Queue checking and task scheduling |
| `hatchet.run/task-assigned-bulk` | Dispatcher | Task routing to workers via gRPC |

### Server Flow

```
API Server → Ingestor → Task Controller → Scheduler → Dispatcher → Worker (SDK)
```

The server-side spans represent internal engine processing and connect to SDK spans when tasks are dispatched to workers.

## Standard Attributes

All Hatchet spans include these attributes with `hatchet.*` prefix:

| Attribute | Type | Description |
|-----------|------|-------------|
| `hatchet.tenant_id` | string | Tenant UUID |
| `hatchet.worker_id` | string | Worker instance ID |
| `hatchet.workflow_run_id` | string | Workflow execution ID |
| `hatchet.step_run_id` | string | Step/Task run external ID |
| `hatchet.action_name` | string | Task name |
| `hatchet.retry_count` | int | Current retry attempt |
| `hatchet.workflow_id` | string | Workflow definition ID |
| `hatchet.workflow_version_id` | string | Workflow version ID |

### Child Workflow Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| `hatchet.parent_workflow_run_id` | string | Parent workflow run ID |
| `hatchet.child_workflow_index` | int | Index in bulk trigger |
| `hatchet.child_workflow_key` | string | Child workflow key |

## Valid Relationships

Edges define valid parent-child span relationships:

### Context Propagation
- `hatchet.run_workflow` → `hatchet.start_step_run` (via traceparent in additionalMetadata)

### Parent-Child Spans
- `hatchet.start_step_run` → `hatchet.run_workflow` (child workflow triggers)
- `hatchet.start_step_run` → `hatchet.push_event` (event publishing)
- `hatchet.start_step_run` → `hatchet.durable.wait_for` (durable waits)

## Design Decisions

**Why `hatchet.*` prefix?**

All Hatchet-originated spans use this prefix to distinguish from user application spans and third-party instrumentation.

**Why CONSUMER for step runs?**

Step runs are triggered by the engine scheduler (queue-based), making CONSUMER the appropriate span kind per OTEL conventions.

**Why context propagation via additionalMetadata?**

Hatchet uses W3C traceparent propagation through the `additionalMetadata` field to maintain trace continuity across service boundaries without coupling to specific transport mechanisms.
