---
id: prom-red-metrics
title: Prom RED metrics
sidebar_label: Prom RED metrics
---

Adds RED request metrics for prometheus

## Parameters

### Create options 
- `metrics`: object [={counter, histogram}] - factories for creating [prom-client](https://www.npmjs.com/package/prom-client) instances
- `labelNames`: array - list of labels for metrics
- `getLabelsValuesFromContext`: function - function for extracting label values from context

### Internal meta
- `TIMER_DONE`: function - function created at the time of sending and called at the end of the request to calculate its duration

## Example

### Base usage

```typescript
import request from '@tinkoff/request-core';
import promRedMetrics from '@tinkoff/request-plugin-prom-red-metrics';
import promClient from 'prom-client';

const req = request([
    promRedMetrics({
        metrics: {
            counter: (options) => new promClient.Counter(options), // here you can mix any of your own parameters
            histogram: (options) => new promClient.Histogram(options),
        },
        labelNames: ['protocol', 'host', 'port'],
        getLabelsValuesFromContext: (context) => context.getRequest()
    }),
    // ...other plugins
]);
```

### Use with @tinkoff/request-plugin-protocol-http

```typescript
import request from '@tinkoff/request-core';
import promRedHttpMetrics from '@tinkoff/request-plugin-prom-red-metrics/lib/httpMetrics';
import http from '@tinkoff/request-plugin-protocol-http';
import promClient from 'prom-client';

const req = request([
    promRedHttpMetrics({
        metrics: {
            counter: (options) => new promClient.Counter(options), // here you can mix any of your own parameters
            histogram: (options) => new promClient.Histogram(options),
        },
        // variables for the http protocol are predefined in the httpMetrics file
    }),
    http(),
]);
```