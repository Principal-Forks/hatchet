# Bulk Event Publishing Telemetry

## Overview

This storyboard documents the telemetry emitted when publishing multiple events in a single batch operation.

## Span: hatchet.bulk_push_event

**Kind:** PRODUCER

This span wraps bulk event publishing via `EventClient.bulkPush()`. More efficient than multiple single pushes when publishing many events.

## Events

### bulk.push.started

Emitted when bulk push operation begins.

**Attributes:**
- `hatchet.tenant_id` - Tenant UUID
- `hatchet.bulk_count` - Number of events being published

### bulk.push.complete

Emitted when all events are published successfully.

**Attributes:**
- `hatchet.bulk_count` - Number of events published

### bulk.push.error

Emitted when bulk push fails.

**Attributes:**
- `hatchet.bulk_count` - Number of events attempted
- `error.type` - Error class/type name
- `error.message` - Error message

## Implementation

- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`
