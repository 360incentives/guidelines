# 360 Europe API standards

These are the set of 'informal' guidelines we have used in the creation of the
MyRewards and GPS APIs. They have never been codified as below before, and as
such may not be followed to the letter by said APIs, though we consider that to
be a 'bug' and would consider rectifying it, in line with our
[versioning](#versioning) policy.

API documentation for above APIs:

- MyRewards: https://docs.my-rewards.co.uk
- GPS: https://docs.gps.my-rewards.co.uk

Of the two, MyRewards should be considered the more mature and thus
'authoritative' regarding these standards.

## Overview

- RESTful API (not RPC etc.)
- JSON-based
- Operate on resources via standard HTTP verbs
  - Idempotent, 'query' endpoints must use `GET`
  - Side-effecting 'command' endpoints must use the relevant non-`GET` verb
- Use standard HTTP response codes
  - 200: default 'success' response
  - 201: when creation of resource is successful
  - 401: missing creds
  - 403: forbidden by domain logic
  - 404: not found in db
- A lot of the following is based on the Rails conventions, as well as that of
  the surrounding Rails/Ruby community
- We make no distinction between "internal" (intra-company-only) and "external"
  APIs - all APIs are considered "external", regardless of their consumers
  - We do have some "internal" APIs within an app, but these are intended to be
    only consumed by the app itself (i.e. from the frontend in the browser) and
    are not subject to versioning etc.

*Standards are followed unless there's a very good reason not to*

## URLs

- Prefer shallow URLs (no nesting)
  - Exceptions:
    - Creation endpoint of a sub-resource (`/widgets/:id/`)
    - Similarly-named resources that exist in different modules of the app
- Uses `under_score` case, all lowercase
- Named after the plural form of the resource in question (e.g. `/widgets`)
  - Unless the resource is conceptually a singleton, then use singular
    (e.g. `/widget`)
- No file extension

## Authentication

- Token-based, via `Authorization` HTTP header
- Tokens generally made up of a key and a secret, joined with a colon (e.g.
  `Authorization: Token token=key:secret`)

## Authorization/Permissions

- Per-key, key linked to a programme ("promotion" in 360 lingo)
- `read` and/or `write` per [resource](#resources)
  - `read` gives access to "index" and "show", plus any other `GET` endpoints
  - `write` gives access to "create", "update" and "destroy", plus any other
    side-effecting non-`GET` endpoints

## JSON format

- When in doubt, follow [Postel's Law](https://en.wikipedia.org/wiki/Robustness_principle)
  ("be conservative in what you send, be liberal in what you accept"). Try to
  make life for the consumer easy, but not at the expense of correctness
- `Content-Type` should be `application/json` for both requests and responses
- Object keys are in lowercased `under_score` format
- Resource representations should contain an `id` with a unique identifier for
  that resource
- Any 'enum'-type field (e.g. "state", "status") should use strings, regardless
  of underlying representation or implementation
- Ask for ISO8601-formatted strings for dates/datetimes, but you may accept
  other suitable formats if they can be parsed unambiguously (*this is largely
  a historical rule, rooted in the UK-centric origin of our apps, and something
  we should probably stop*)
  - Return ISO8601-formatted strings for dates/datetimes, regardless of stack or
    consumer locale
  - ...with UTC timezone? (this needs to be checked as to whether we do this)
- Prefer arrays of objects (with an `id` key) for collections of things, over
  objects keyed by `id` or another property

## Versioning

- Per endpoint, not per API
- Monotonically increasing integer, at root of path (`/v1/`, `/v2/` etc.)
- Only increased on "breaking changes" - non-backwards-compatible changes to
  the semantics of the endpoint:
  - Field renamed in request or response
  - Field removed
  - Format of URL parameters changed
- Whenever possible, changes should be backwards-compatible and "grow-only":
  - Retain field under old name when renamed/removed
  - Accept 'old' and 'new' formats, when not too onerous on the consumer or
    server
- Old versions of an endpoint should continue to exist along the newest
  version, and be supported until consumers are migrated to newer versions

*N.B. we have never retired an endpoint in the MyRewards or GPS APIs, though we
have deprecated a few*

## Resources

- "index" (`GET /widgets`)
- "create" (`POST /widgets`)
- "show" (`GET /widgets/:id`)
- "update" (`PUT /widgets/:id` or `PATCH /widgets/:id`)
- "destroy" (`DELETE /widgets/:id`)

### Applicable to all singular endpoints (with identifier in URL, e.g. "show", "update", "destroy")

- Returns 404 if no resource exists with given identifier
- Returns 403 if consumer does not have permission to access and/or delete
  resource
- Returns 404 if existence of resource is considered sensitive information

### Error responses

- When an error condition cannot be described by the HTTP status alone, return
  a JSON object with the following format:

```json
{
  "errors": ["Error description", "Another error"]
}
```

- `errors` should always be an array, even in the case of a single error
- Error messages should be human-readable and helpfully descriptive (e.g.
  "Count should be an integer" vs. "Count is invalid")
  - Vagueness for the sake of security should be the only exception

### "index"

- Returns JSON array of objects, corresponding to the response in question
- Filtering is done via HTTP query parameters

### "create"

- Accepts JSON object representation of resource, and returns the same,
  normalized/formatted as required (e.g. default fields filled in)
  - Returns `422` response if submitted object fails validation, using the standard
    [error format](#error-responses)
- Endpoints accepting multiple resources are allowed (this is a pragmatic
  diversion from 'pure' REST), but should be separate from the
  singular-resource-accepting endpoint

### "show"

- Returns JSON object representation of resource
- `:id` must be a unique identifier for that resource within its collection
  - Generally the primary key from the corresponding database table
  - May be another form of domain-specific identifier where that makes sense
  - IDs should be uniform in format (e.g. integers, UUIDs)
    - If another identifier/format is needed, a separate nested endpoint should
      be provided (e.g. `/widgets/by_email/:email`)
- Includes sub-resources where that makes sense - e.g. answers to a claim form
  within a claim
- When possible, response should be able to be sent to the "update" endpoint
  without changes to the JSON structure

### "update"

- Accepts partial JSON object representation of resource, and returns full
  representation (this should be the same representation as ["show"](#show))
  - Only send fields that need to be changed, but accept other fields
- Response should ideally be able to sent back to the endpoint unchanged in
  JSON structure

#### State changes

- Prefer separate `POST` endpoints to enact state changes
  - e.g. `POST /widgets/1/approvals`, not `PATCH /widgets/1` with body
    `{"state":"active"}`
  - Abstracts underlying db representation and allows us to make changes
    without having to change interface

### "destroy"

- URL follows same format and rules as ["show" endpoint](#show)
- No request body should be sent
- Returns 204 ("No Content") on successful deletion, with no response body
