### Abstract

Deplon is a deployment tool aims to maximize developer experience (DX) and minimize time spent in DevOps.
A Deplon system consists of one or more worker node and single control node.
Usually worker nodes don't have information about control node and they do not ask control node for data.
So when a deployment is performed, the control node must order desired worker node to update required informations.
This procedure allows control node to not be on a fixed machine and to be offline when they are not performing any deployment or updates.
Control node not being on a fixed machine makes Deplon works with existing CI/CD tools like GitHub Actions, lowering or even removing operation cost of maintaining always online machine and building deployment images.

### REST API Specification

#### GET /api/deployments/list
Lists deployments stored on the worker node.
The control node can compare the list between deployments of target worker node and itself to make up update list to use at /api/deployments/update

**Request queries**
- full: bool (default: false)
  if false, response element only contains id, updated_at (and hash if hash=true).
  if true, response element contains full configuration.
- hash: string? (default: null)
  describes hash algorithm to hash the configuration.
  if specified, 'full' search query must be true.

**Response JSON schema**
```
[
  {
    "id": "(target deployment id)",
    "updated_at": $unixtime
  }
]
```

**Response JSON schema** (full=true)
```
[
  {
    "id": "(target deployment id)",
    "configuration": "(deployment configuration toml)",
    "updated_at": $unixtime
  },
  ...
]
```

**Response JSON schema (full=true,hash=true)**
```
[
  {
    "id": "(target deployment id)",
    "hash": "hash(deployment configuration toml)",
    "updated_at": $unixtime
  }
]
```

#### POST /api/deployments/update
Updates deployments configuration of target machine.

**Request JSON schema**

```
[
  {
    "id": "(target deployment id)",
    "configuration": "(deployment configuration toml)",
    "updated_at": $unixtime
  },
  ...
]
```

The worker node implementation can use updated_at to decide whether it should or should not update the stored deployment configuration.
The request list may not contain full deployments configuration of the control node if it had have list of worker node configurations and there is no visible changes to reduce traffic on massive deployments system.
