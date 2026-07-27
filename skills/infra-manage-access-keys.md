---
name: Issue and manage Infra access keys
description: Create an access key for CI/CD or API automation, list existing keys, and
  revoke a key when it is no longer needed.
api: openapi/infra-openapi-original.json
operations: [CreateAccessKey, ListAccessKeys, DeleteAccessKey, DeleteAccessKeys]
---

# Manage access keys

Access keys are bearer credentials issued for an identity; they inherit that identity's
permissions and carry an expiry and inactivity timeout.

## Prerequisites
- Authenticate with `Authorization: Bearer <ACCESSKEY>` and the `Infra-Version` header.

## Steps
1. Create a key. Call `CreateAccessKey` with a `name`, the `issuedForID` (the user the
   key acts as), and optionally `expiry` / `inactivityTimeout`.
2. Capture the secret. The `accessKey` value in the `CreateAccessKeyResponse` is shown
   **once** — store it securely. Later calls only return metadata.
3. Audit. Call `ListAccessKeys` (filter by `userID` or `name`) to review issued keys,
   their `expires`, and `issuedForKind`.
4. Revoke. Call `DeleteAccessKey` with the key `id`, or `DeleteAccessKeys` with a `name`
   to remove by name.

## Conventions
- Treat the `accessKey` secret like a password; it grants the issuing identity's access.
- Errors return `{ code, message, fieldErrors[] }`. A 401 usually means the caller's own
  key is missing/expired or the `Infra-Version` header is absent.
