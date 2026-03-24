# Bulk Workflow Trigger Telemetry

## Overview

This storyboard documents the telemetry emitted when triggering multiple workflows in a single batch operation.

## Span: hatchet.run_workflows

**Kind:** PRODUCER

This span wraps bulk workflow triggering via `AdminClient.runWorkflows()`. It's more efficient than multiple single triggers when launching many workflow instances.

## Events

### bulk.trigger.started

Emitted when bulk trigger operation begins.

**Attributes:**
- `hatchet.workflow_name` - Name of the workflow being triggered
- `hatchet.tenant_id` - Tenant UUID
- `hatchet.bulk_count` - Number of workflows being triggered

### bulk.trigger.complete

Emitted when all workflows are triggered successfully.

**Attributes:**
- `hatchet.workflow_name` - Name of the triggered workflow
- `hatchet.bulk_count` - Number of workflows triggered

### bulk.trigger.error

Emitted when bulk trigger fails.

**Attributes:**
- `hatchet.workflow_name` - Name of workflow that failed
- `error.type` - Error class/type name
- `error.message` - Error message

## Implementation

- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`
