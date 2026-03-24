# Ingestor Telemetry

## Overview

This storyboard documents the telemetry emitted by the Ingestor service, which handles event ingestion that triggers workflows.

## Spans

### hatchet.run/ingest-event

**Kind:** INTERNAL

Processes single event ingestion.

### hatchet.run/ingest-events

**Kind:** INTERNAL

Processes multiple events (batch variant).

### hatchet.run/bulk-ingest-event

**Kind:** INTERNAL

Processes bulk event ingestion API call.

### hatchet.run/ingest-replayed-event

**Kind:** INTERNAL

Processes replayed events (re-triggering workflows from historical events).

## Common Attributes

- `hatchet.run/tenant.id` - Tenant UUID
- `hatchet.run/event.id` - Event UUID (for single events)

## Implementation

- **Ingestor:** `internal/services/ingestor/ingestor_v1.go`
