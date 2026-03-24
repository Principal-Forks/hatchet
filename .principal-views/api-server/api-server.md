# API Server HTTP Telemetry

## Overview

This storyboard documents the HTTP request telemetry emitted by the Hatchet API server. Uses otelecho middleware for automatic instrumentation.

## Span: HTTP Routes

**Kind:** SERVER

HTTP spans are automatically created by the `otelecho.Middleware` for all incoming requests. Span names follow the pattern `{METHOD} {route}` (e.g., `GET /api/workflows`).

### Skipped Routes

Health check endpoints are excluded from tracing:
- `/api/ready` (readiness probe)
- `/api/live` (liveness probe)

## Events

### http.request.started

Implicit event when HTTP request is received. Attributes set on span creation.

**Attributes:**
- `http.method` - HTTP method (GET, POST, PUT, DELETE, etc.)
- `http.route` - Route pattern
- `hatchet.run/tenant.id` - Tenant UUID (when authenticated)

### http.request.complete

HTTP request completed with success status (2xx/3xx).

**Attributes:**
- `http.status_code` - Response status code

### http.request.error

HTTP request failed with error status (4xx/5xx). Custom `ErrorStatusMiddleware` extends otelecho to mark 4xx as errors (otelecho only marks 5xx by default).

**Attributes:**
- `http.status_code` - Response status code
- `error.message` - Error details (if available)

## Implementation

- **Middleware:** `api/v1/server/middleware/telemetry/telemetry.go`
- **Error Handling:** Custom middleware marks 4xx and 5xx as error spans
