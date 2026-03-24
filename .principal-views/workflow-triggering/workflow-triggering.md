# Workflow Triggering Telemetry

## Overview

This storyboard documents the telemetry emitted when triggering workflows programmatically via the SDK.

## Spans

### hatchet.run_workflow

**Kind:** PRODUCER

Emitted when triggering a single workflow via `AdminClient.runWorkflow()`.

### hatchet.run_workflows

**Kind:** PRODUCER

Emitted when triggering multiple workflows via `AdminClient.runWorkflows()`.

## Events

### workflow.trigger.started

Emitted when a workflow trigger operation begins.

**Attributes:**
- `hatchet.workflow_name` - Name of the workflow being triggered
- `hatchet.tenant_id` - Tenant UUID

### workflow.trigger.complete

Emitted when workflow is triggered successfully.

**Attributes:**
- `hatchet.workflow_name` - Name of the triggered workflow
- `hatchet.workflow_run_id` - ID of the created workflow run

### workflow.trigger.error

Emitted when workflow trigger fails.

**Attributes:**
- `hatchet.workflow_name` - Name of the workflow that failed
- `error.type` - Error class/type name
- `error.message` - Error message

## Context Propagation

When triggering workflows from within a step function, the current trace context is propagated via the `additionalMetadata` field. This enables distributed tracing across parent-child workflow relationships.

## Implementation

- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`
- **Go SDK:** `sdks/go/opentelemetry/middleware.go`

The instrumentation patches `AdminClient.runWorkflow()` and `AdminClient.runWorkflows()` methods.
