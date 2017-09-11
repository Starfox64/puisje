# Puis-je ? (May I?)
Puis-je is a simple yet powerful authorization library designed with dynamic customization in mind.

[![MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/Starfox64/puisje/blob/master/LICENSE)
[![npm](https://img.shields.io/npm/v/puisje.svg)](https://www.npmjs.com/package/puisje)
[![npm](https://img.shields.io/npm/dt/puisje.svg)](https://www.npmjs.com/package/puisje)

## Features
- [ ] Role Inheritance
- [ ] Permission

## Core Principles
In Puis-je there are 4 things: Roles, Permissions, AuthContexts and various kinds of DataStores.

### Roles
A role is a list of permission that can either be:
- **Permanently Granted** (inheritance will be ignored)
- **Granted**
- **Unchanged**
- **Denied**
- **Permanently Denied** (inheritance will be ignored)

A role can also inherit from another role, roles that do not inherit from any roles will inherit from the `@` role.
Think of this role as the root of the inheritance tree or just a role representing everyone.
This role has an additional property: `defaultPolicy` which defaults to **Denied** but can be changed to **Granted**.

### Permissions
A permission is what grants access to a resource, they are mere strings and do not have to be "registered" anywhere.
Here are a few permission examples:
- `storage:user:create`
- `storage:user:read`
- `storage:user:delete`
- `storage:user` Targets `storage:user:create`, `storage:user:read` and `storage:user:delete`
- `storage:file:create`
- `storage:file:read`
- `storage:file:delete`
- `storage:file` Targets `storage:file:create`, `storage:file:read` and `storage:file:delete`
- `storage` Targets both `storage:user` and `storage:file`

To avoid long lists of permissions you can also use the summarization notation:
- `storage:user:create`
- `storage:user:read`
- `storage:user:delete`

Becomes:
- `storage:user>create,read,delete`

Keep in mind that if you were to have a `storage:user:delete:bulk` permission, the previous summarization notation would also grant access to it.
A permission can only contain case insensitive alphanumeric characters, whitespace characters will automatically be removed.

#### Permission Precedence
Permission precedence means that the permissions with the least amount of tokens (names separated by _:_ or _>_) are applied first and the ones with most amount of tokens are applied last.
Two permissions with the same precedence will be applied in the order they are declared.

Example:
- **Grant** `storage:user:read`
- **Deny** `storage:user`
- **Deny** `storage:file>read,delete`
- **Grant** `storage:file:create`
- **Grant** `storage:file:read`
- **Deny** `storage` (effectively useless unless you are overriding the default policy)

Means:
- **Deny** `storage:user:create`
- **Grant** `storage:user:read`
- **Deny** `storage:user:delete`
- **Grant** `storage:file:create`
- **Grant** `storage:file:read`
- **Deny** `storage:file:delete`

### AuthContexts
An authorization context is simply an object listing a user (or anything else)'s roles and directly assigned permissions.
The permissions directly defined in an authorization context have the highest priority behind permissions granted/denined permanently.

### DataStores
Data stores are adapters allowing you to fetch data like roles, inheritance tree cache, etc... from your storage solution of choice:
- [ ] InMemory [Roles, Cache]
- [ ] MongoDB [Roles]
- [ ] MySQL [Roles]
- [ ] SQLite [Roles]
- [ ] Redis [Cache]
