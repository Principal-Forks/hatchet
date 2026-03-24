# Event Publishing Telemetry

## Overview

This storyboard documents the telemetry emitted during event publishing operations. Events are the primary mechanism for triggering workflows in Hatchet.

## Spans

### hatchet.push_event

**Kind:** PRODUCER

Emitted when publishing a single event via `EventClient.push()`.

### hatchet.bulk_push_event

**Kind:** PRODUCER

Emitted when publishing multiple events via `EventClient.bulkPush()`.

## Events

### event.push.started

Emitted when an event publishing operation begins.

**Attributes:**
- `hatchet.event_key` - The event type/key being published
- `hatchet.tenant_id` - Tenant UUID

### event.push.complete

Emitted when event publishing completes successfully.

**Attributes:**
- `hatchet.event_key` - The event type/key that was published

### event.push.error

Emitted when event publishing fails.

**Attributes:**
- `hatchet.event_key` - The event type/key that failed
- `error.type` - Error class/type name
- `error.message` - Error message

## Implementation

- **TypeScript SDK:** `sdks/typescript/src/opentelemetry/instrumentor.ts`
- **Go SDK:** `sdks/go/opentelemetry/middleware.go`

The instrumentation patches `EventClient.push()` and `EventClient.bulkPush()` methods to wrap them with spans.
