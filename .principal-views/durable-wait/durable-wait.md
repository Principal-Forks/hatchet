# Durable Wait Telemetry

## Overview

This storyboard documents the telemetry emitted during durable context wait operations.

## Span: hatchet.durable.wait_for

**Kind:** INTERNAL

This span wraps durable wait operations via `DurableContext.waitFor()`. Durable waits allow a step to pause execution and wait for an external signal or condition before resuming.

## Events

### wait.started

Emitted when a durable wait begins.

**Attributes:**
- `hatchet.wait_key` - Key identifying what is being waited for
- `hatchet.step_run_id` - Step run performing the wait

### wait.complete

Emitted when the wait condition is satisfied and execution resumes.

**Attributes:**
- `hatchet.wait_key` - Key that was waited for

### wait.error

Emitted when the wait fails or times out.

**Attributes:**
- `hatchet.wait_key` - Key that was being waited for
- `error.type` - Error class/type name
- `error.message` - Error message

## Use Cases

- Waiting for human approval
- Waiting for external webhook
- Waiting for child workflow completion
- Waiting for time-based conditions

## Implementation

- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`
