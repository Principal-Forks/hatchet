# Scheduler Telemetry

## Overview

This storyboard documents the telemetry emitted by the Scheduler service, which determines when and where tasks should run.

## Spans

All spans extract parent context from message carriers via `NewSpanWithCarrier`.

### hatchet.run/handle-check-queue

**Kind:** INTERNAL

Checks queue for tasks that are ready to be scheduled.

### hatchet.run/handle-new-worker

**Kind:** INTERNAL

Processes new worker registration, potentially scheduling pending tasks.

### hatchet.run/handle-new-queue

**Kind:** INTERNAL

Processes new queue creation.

### hatchet.run/handle-new-concurrency-strategy

**Kind:** INTERNAL

Processes concurrency strategy updates.

### hatchet.run/schedule-step-runs

**Kind:** INTERNAL

Performs actual scheduling of step runs to workers.

## Context Propagation

All message handlers extract parent context from `msg.OtelCarrier` for distributed tracing continuity.

## Implementation

- **Scheduler:** `internal/services/scheduler/v1/scheduler.go`
