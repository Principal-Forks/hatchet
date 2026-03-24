# Hatchet Service Resources & Scopes

## Overview

This document describes the OpenTelemetry resources and instrumentation scopes for Hatchet's distributed task orchestration system.

## Resources

| Resource | Service Name | Description |
|----------|--------------|-------------|
| `hatchet-worker` | `hatchet-worker` | Worker processes running step functions via SDK |
| `hatchet-api` | `hatchet-api` | REST API server for HTTP endpoints |
| `hatchet-engine` | `hatchet-engine` | Task scheduling, coordination, and OTLP collection |

### Resource Attributes

All Hatchet services emit these standard resource attributes:

| Attribute | Example | Description |
|-----------|---------|-------------|
| `service.name` | `hatchet-worker` | Logical service identifier |
| `library.language` | `go`, `typescript` | SDK language |
| `k8s.pod.name` | `worker-abc123` | Kubernetes pod (when deployed to K8s) |
| `k8s.namespace.name` | `hatchet` | Kubernetes namespace |

## Instrumentation Scopes

Workers can be instrumented via either SDK:

| Scope | Language | Package |
|-------|----------|---------|
| Go SDK | Go | `github.com/hatchet-dev/hatchet/sdks/go/opentelemetry` |
| TypeScript SDK | TypeScript | `@hatchet-dev/typescript-sdk` |

## Design Decisions

**Why separate worker resource from API/Engine?**

Workers run user-defined step functions and may be deployed across many instances. The API and Engine are central infrastructure components. Separating them allows independent scaling and monitoring.

**Why two instrumentation scopes?**

Hatchet supports both Go and TypeScript SDKs. Each has its own instrumentation implementation that wraps SDK methods with spans and propagates context.
