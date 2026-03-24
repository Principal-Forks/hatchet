# Hatchet Instrumentation Scopes

## Overview

Hatchet provides two instrumentation scopes - one for Go SDK workers and one for TypeScript SDK workers. Both implement the same telemetry patterns but in their respective languages.

## Scopes

### Go SDK

**Scope:** `github.com/hatchet-dev/hatchet/sdks/go/opentelemetry`

**Files:**
- `sdks/go/opentelemetry/instrumentor.go` - Tracer provider setup
- `sdks/go/opentelemetry/middleware.go` - Span creation middleware
- `sdks/go/opentelemetry/span_processor.go` - Attribute injection

**Features:**
- Middleware-based span wrapping for step execution
- Automatic traceparent extraction from additionalMetadata
- HatchetAttributeSpanProcessor for automatic attribute injection
- Context propagation to child spans

### TypeScript SDK

**Scope:** `@hatchet-dev/typescript-sdk`

**Files:**
- `sdks/typescript/src/opentelemetry/instrumentor.ts` - Instrumentation hooks

**Features:**
- InstrumentationBase subclass patching SDK methods
- AsyncLocalStorage for context propagation
- Patches EventClient, AdminClient, InternalWorker, ScheduleClient, DurableContext
- Automatic attribute injection via attribute map

## Design Decisions

**Why separate scopes per language?**

Each SDK has its own instrumentation implementation with language-specific patterns (Go middleware vs. TypeScript instrumentation hooks). Separate scopes allow tracing which SDK version emitted the telemetry.

**Why automatic attribute injection?**

The HatchetAttributeSpanProcessor (Go) and equivalent TypeScript mechanism ensure all child spans created during step execution inherit `hatchet.*` attributes, enabling correlation without manual instrumentation.
