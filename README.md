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
- Permanently Granted (If Permanently Granted, inheritance will be ignored)
- Granted
- Unchanged
- Denied
- Permanently Denied (If Permanently Denied, inheritance will be ignored)

A role can also inherit from another role, roles that do not inherit from any roles will inherit from the `@` role.
Think of this role as the root of the inheritance tree or just a role representing everyone.

### Permissions
A permission is what grants access to a resource, they are mere strings and do not have to be "registered" anywhere.
Here are a few permission examples:
- `storage.user.create`
- `storage.user.read`
- `storage.user.delete`
- `storage.user` Grants access to `storage.user.create`, `storage.user.read` and `storage.user.delete`
- `storage.file.create`
- `storage.file.read`
- `storage.file.delete`
- `storage.file` Grants access to `storage.file.create`, `storage.file.read` and `storage.file.delete`
- `storage` Grants access to both `storage.user` and `storage.file`

To avoid long lists of permissions you can also use the summarization notation:
- `storage.user.create`
- `storage.user.read`
- `storage.user.delete`

Becomes:
- `storage.user:create,read,delete`

Keep in mind that if you were to have a `storage.user.delete.bulk` permission, the previous summarization notation would not grant access to it, you would have to change it to:
- `storage.user:create,read,delete,delete.bulk`

<!-- TODO: Add example of permission denial priority -->

A permission can only contain characters 0-9, a-z and A-Z.

### AuthContexts
An authorization context is simply an object listing a user (or anything else)'s groups and directly assigned permissions.
The permissions directly defined in an authorization context have the highest priority behind permissions granted/denined permanently.

### DataStores
Data stores are adapters allowing you to fetch data like roles, inheritance tree cache, etc... from your storage solution of choice:
- [ ] InMemory [Roles, Cache]
- [ ] MongoDB [Roles]
- [ ] MySQL [Roles]
- [ ] SQLite [Roles]
- [ ] Redis [Cache]
