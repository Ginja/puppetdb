---
title: "PuppetDB 1.6 » API » v4 » Querying Aggregate Event Counts"
layout: default
canonical: "/puppetdb/latest/api/query/v4/aggregate-event-counts.html"
---

[event-counts]: ./event-counts.html
[curl]: ../curl.html

> **Note:** The v4 API is experimental and may change without notice. For stability, it is recommended that you use the v3 API instead.

## Routes

### `GET /v4/aggregate-event-counts`

This will return aggregated count information about all of the resource events matching the given query.
This endpoint is built entirely on the [`event-counts`][event-counts] endpoint and will aggregate those
results into a single map.

#### Parameters

This endpoint builds on top of the [`event-counts`][event-counts] endpoint and will forward to it all
parameters. Supported parameters are listed below for reference.

* `query`: Required. A JSON array of query predicates in prefix form (`["<OPERATOR>", "<FIELD>", "<VALUE>"]`).
This query is forwarded to the [`event-counts`][event-counts] endpoint - see there for additional documentation.

* `summarize-by`: Required. A string specifying which type of object you'd like count. Supported values are
`resource`, `containing-class`, and `certname`.

* `count-by`: Optional. A string specifying what type of object is counted when building up the counts of
`successes`, `failures`, `noops`, and `skips`. Supported values are `resource` (default) and `certname`.

* `counts-filter`: Optional. A JSON array of query predicates in the usual prefix form. This query is applied to
the final event-counts output, but before the results are aggregated. Supported operators are `=`, `>`, `<`,
`>=`, and `<=`. Supported fields are `failures`, `successes`, `noops`, and `skips`.

* `distinct-resources`: Optional.  (EXPERIMENTAL: it is possible that the behavior
of this parameter may change in future releases.)  This parameter is passed along
to the [`event`][events] query - see there for additional documentation.

##### Operators

This endpoint builds on top of the [`event-counts`][event-counts] and therefore supports all of the same operators.

##### Fields

This endpoint builds on top of the [`event-counts`][event-counts] and therefore supports all of the same fields.

#### Response Format

The response is a single JSON map containing aggregated event-count information and a `total` for how many
event-count results were aggregated.

    {
      "successes": 2,
      "failures": 0,
      "noops": 0,
      "skips": 1,
      "total": 3
    }

#### Paging

This endpoint always returns a single result so paging is not necessary.

#### Example

You can use [`curl`][curl] to query information about aggregated resource event counts like so:

    curl -G 'http://localhost:8080/v4/aggregate-event-counts'
            --data-urlencode 'query=["=", "certname", "foo.local"]' \
            --data-urlencode 'summarize-by=containing-class'
