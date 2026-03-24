# Cancel Step Run Telemetry

## Overview

This storyboard documents the telemetry emitted when handling step cancellation requests from the engine.

## Span: hatchet.cancel_step_run

**Kind:** CONSUMER

This span is created when a worker receives a cancellation request for a running step. The cancellation may be triggered by workflow timeout, manual cancellation, or parent workflow termination.

## Events

### cancel.started

Emitted when cancellation handling begins.

**Attributes:**
- `hatchet.step_run_id` - Step run being cancelled
- `hatchet.tenant_id` - Tenant UUID

### cancel.complete

Emitted when cancellation completes successfully.

**Attributes:**
- `hatchet.step_run_id` - Step run that was cancelled

### cancel.error

Emitted when cancellation fails.

**Attributes:**
- `hatchet.step_run_id` - Step run that failed to cancel
- `error.type` - Error class/type name
- `error.message` - Error message

## Implementation

- **Go SDK:** `sdks/go/opentelemetry/middleware.go`
- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`
