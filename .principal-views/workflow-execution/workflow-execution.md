# Workflow Execution Telemetry

## Overview

This storyboard documents the telemetry emitted during workflow step execution. The primary span is `hatchet.start_step_run` which wraps the execution of user-defined step functions.

## Span: hatchet.start_step_run

**Kind:** CONSUMER

This span is created when a worker receives a step run assignment from the engine. It captures the full lifecycle of step execution.

## Events

### step.started

Emitted when step execution begins. The span is created with all Hatchet context attributes extracted from the incoming message.

**Attributes:**
- `hatchet.tenant_id` - Tenant UUID
- `hatchet.worker_id` - Worker instance ID
- `hatchet.workflow_run_id` - Workflow execution ID
- `hatchet.step_run_id` - Step run external ID (critical for lookup)
- `hatchet.action_name` - Task/action name
- `hatchet.retry_count` - Current retry attempt (0 for first attempt)

### step.complete

Emitted when step execution completes successfully. The span status is set to OK.

### step.error

Emitted when step execution fails. The span status is set to ERROR and the exception is recorded.

**Attributes:**
- `error.type` - Error class/type name
- `error.message` - Error message

## Context Propagation

The span extracts W3C traceparent from `additionalMetadata` to maintain distributed trace continuity. Child spans created during step execution (e.g., `hatchet.run_workflow`, `hatchet.push_event`) inherit the parent context.

## Implementation

- **Go SDK:** `sdks/go/opentelemetry/middleware.go`
- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`

The `HatchetAttributeSpanProcessor` automatically injects `hatchet.*` attributes into all child spans created within the step context.
