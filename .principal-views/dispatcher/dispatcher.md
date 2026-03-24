# Dispatcher Telemetry

## Overview

This storyboard documents the telemetry emitted by the Dispatcher service, which routes tasks to workers via gRPC streams.

## Spans

### hatchet.run/task-assigned-bulk

**Kind:** INTERNAL

Processes bulk task assignment messages. Extracts parent context from message carrier for distributed tracing.

### hatchet.run/send-to-worker

**Kind:** INTERNAL

Sends task to worker via gRPC stream. Child spans include:
- `encode-action` - Encoding payload
- `acquire-worker-stream-lock` - Lock acquisition
- `send-worker-stream` - Stream write

### hatchet.run/tasks-cancelled

**Kind:** INTERNAL

Processes batch task cancellation.

## Context Propagation

The dispatcher extracts parent context from message carriers (`msg.OtelCarrier`) to maintain trace continuity from scheduler to dispatcher to worker.

## Error Handling

Flow control errors are captured:
- `SetStatus(codes.Error, "flow control is active")`
- `SetStatus(codes.Error, "flow control detected")`

## Implementation

- **Dispatcher:** `internal/services/dispatcher/dispatcher_v1.go`
- **Worker Communication:** `internal/services/dispatcher/subscribed_worker_v1.go`
