# Task Controller Telemetry

## Overview

This storyboard documents the telemetry emitted by the Task Controller service, which handles task state transitions (completed, failed, cancelled).

## Spans

All spans use the `hatchet.run/` prefix convention.

### hatchet.run/TasksControllerImpl.handleTaskCompleted

**Kind:** INTERNAL

Handles task completion messages from workers.

### hatchet.run/TasksControllerImpl.handleTaskFailed

**Kind:** INTERNAL

Handles task failure messages from workers. Records error on span if completion fails.

### hatchet.run/TasksControllerImpl.handleTaskCancelled

**Kind:** INTERNAL

Handles task cancellation messages.

### hatchet.run/TasksControllerImpl.handleProcessUserEvents

**Kind:** INTERNAL

Processes user-generated events.

### hatchet.run/TasksControllerImpl.handleProcessInternalEvents

**Kind:** INTERNAL

Processes internal system events.

## Common Attributes

All spans include:
- `hatchet.run/tenant.id` - Tenant UUID

## Implementation

- **Controller:** `internal/services/controllers/task/controller.go`
