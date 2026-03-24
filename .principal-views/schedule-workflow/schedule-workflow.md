# Schedule Workflow Telemetry

## Overview

This storyboard documents the telemetry emitted when creating scheduled workflow executions.

## Span: hatchet.schedule_workflow

**Kind:** PRODUCER

This span wraps workflow scheduling via `ScheduleClient`. Scheduled workflows are executed at a specified future time or on a recurring schedule.

## Events

### schedule.started

Emitted when workflow scheduling begins.

**Attributes:**
- `hatchet.workflow_name` - Name of the workflow being scheduled
- `hatchet.tenant_id` - Tenant UUID

### schedule.complete

Emitted when workflow is scheduled successfully.

**Attributes:**
- `hatchet.workflow_name` - Name of the scheduled workflow

### schedule.error

Emitted when workflow scheduling fails.

**Attributes:**
- `hatchet.workflow_name` - Name of workflow that failed to schedule
- `error.type` - Error class/type name
- `error.message` - Error message

## Implementation

- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`
