---
name: Onboard a user and place them in a group with access
description: Create a user identity, add them to a group, and grant the group access to
  a destination so access is managed by group membership.
api: openapi/infra-openapi-original.json
operations: [CreateUser, ListGroups, CreateGroup, UpdateUsersInGroup, CreateGrant]
---

# Onboard an identity

Prefer granting access to groups, then managing people by group membership.

## Prerequisites
- Authenticate with `Authorization: Bearer <ACCESSKEY>` and the `Infra-Version` header.

## Steps
1. Create the user. Call `CreateUser` with a `name` (email). Capture the returned `id`.
2. Find or create the group. Call `ListGroups` (filter by `name`); if missing, call
   `CreateGroup`.
3. Add the user to the group. Call `UpdateUsersInGroup` (`PATCH /api/groups/{id}/users`)
   with the user id in `usersToAdd`.
4. Grant the group access. Call `CreateGrant` with the `group` id, a `privilege`, and a
   `resource` (destination). All current and future group members inherit it.

## Conventions
- New users authenticate via a configured OIDC `Provider` or password credentials; see
  authentication/infra-authentication.yml.
- Errors return `{ code, message, fieldErrors[] }`; a 409 on `CreateUser`/`CreateGroup`
  means the name already exists — reuse it via the list call.
