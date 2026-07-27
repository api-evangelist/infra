---
name: Grant a user or group access to a destination
description: Give an identity least-privilege access to a Kubernetes cluster, server,
  or database managed by Infra, then verify the grant.
api: openapi/infra-openapi-original.json
operations: [ListDestinations, ListGrants, CreateGrant, DeleteGrant]
---

# Grant access with Infra

Use this to bind a user or group to a privilege on a destination resource.

## Prerequisites
- An organization-scoped access key. Send it on every request as `Authorization: Bearer <ACCESSKEY>`.
- The `Infra-Version` header on every request (e.g. `Infra-Version: 0.18.1`).

## Steps
1. Find the destination. Call `ListDestinations` and pick the target by `name`/`kind`.
   Note its resources and roles — the grant's `resource` and `privilege` come from these.
2. (Optional) Check existing access. Call `ListGrants` filtered by `resource` and/or
   `user`/`group` to avoid duplicate grants.
3. Create the grant. Call `CreateGrant` with the subject (`user` id **or** `group` id),
   the `privilege` (a role the destination exposes, e.g. `cluster-admin`, `view`), and
   the `resource` (the destination name, optionally `destination.namespace`).
4. Verify. Call `ListGrants` again filtered by the same `resource` and confirm the new grant.
5. To revoke later, call `DeleteGrant` with the grant `id`.

## Conventions
- Pagination on list calls is `page`/`limit`; responses carry `totalCount`/`totalPages`.
- Errors return `{ code, message, fieldErrors[] }` (not RFC 9457). A 403 means the
  caller's own grants are insufficient; a 409 means the grant already exists.
- No idempotency key — guard against duplicates with the `ListGrants` check in step 2.
