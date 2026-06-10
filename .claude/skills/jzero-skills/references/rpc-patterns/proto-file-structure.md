# Proto File Structure

## Overview

jzero supports multi-proto management (goctl native tool does not support this). When automatically generating code, jzero automatically recognizes files under `desc/proto/` and automatically registers them to zrpc.

jzero also supports proto field validation by default.

In `wownow-micro`, `desc/proto/` is not the long-term source of truth for every proto. Shared RPC contracts are maintained under `contracts/proto/` and copied into service `desc/proto/` only when generation needs a local file layout.

## jzero Framework Philosophy

**Different modules should be separated into different proto files**

## Proto File Standards

### Import Rule
Based on go-zero's proto standard: In service RPC methods, input and output parameters' proto messages cannot be from imported proto files - they must be defined in the current file only.

## Proto File Example

```protobuf
syntax = "proto3";

package version;

import "google/api/annotations.proto";
import "grpc-gateway/protoc-gen-openapiv2/options/annotations.proto";

option go_package = "./types/version";

message VersionRequest {}

message VersionResponse {
  string version = 1;
  string goVersion = 2;
  string commit = 3;
  string date = 4;
}

service Version {
  // Version 获取服务版本信息
  rpc Version(VersionRequest) returns(VersionResponse) {
    option (google.api.http) = {
      get: "/version"
    };
  };
}
```

## File Structure

**Required elements:**

1. **Syntax declaration**: `syntax = "proto3";`
2. **Package name**: Unique identifier for the proto file
3. **Imports**:
   - `google/api/annotations.proto` - For HTTP mapping
   - `grpc-gateway/protoc-gen-openapiv2/options/annotations.proto` - For OpenAPI documentation
   - Custom imports as needed
4. **Go package option**: `option go_package = "./types/version";`
5. **Messages**: Request and response structures
6. **Service** (✅ 必须): RPC method definitions, 必须定义 service
7. **RPC Methods** (✅ 必须 + 注释): 每个 RPC 方法必须添加注释说明其功能

## Repository Pattern: Central Contracts + Service Hooks

In this repository, prefer the following layout:

```text
contracts/
├── proto/
│   ├── <domain>/              # shared business RPC source protos
│   ├── third_party/           # shared imported dependencies
│   └── version/version.proto  # shared version RPC source proto
apps/service/<svc>/
├── .jzero.yaml                # pre/post hooks copy shared protos into desc/proto/
└── desc/proto/                # temporary local generation inputs
```

Rules:

1. Keep canonical RPC source protos in `contracts/proto/`.
2. Let `scripts/jzero-pre-gen.sh` copy required contract protos and `version.proto` into the service `desc/proto/` before `jzero gen`.
3. Let `scripts/jzero-post-gen.sh` clean up those temporary copies after generation.
4. Do not hand-maintain duplicated service-local `desc/proto/version.proto`.
5. Service-local generated protobuf code can still target `internal/types/...` when the proto `go_package` is intentionally relative.
6. Generate shared protobuf/grpc Go code into `contracts/gen/**` with `make proto`.
7. Go BFFs consume `contracts/gen/**` directly and should keep only thin `internal/rpc/<domain>/clientset.go` packages.
8. Required RPC clients should be wired in `ServiceContext` startup, not hidden behind runtime `svcCtx.<Domain> != nil` checks in logic code.
9. Once a `Clientset` exists, its accessors are expected to return usable generated grpc clients without extra accessor-level nil guards.

## Best Practices

### ✅ Correct Patterns

```protobuf
// Define request/response in the same file as the service
syntax = "proto3";

package user;

option go_package = "./types/user";

message CreateUserRequest {
  string name = 1;
  string email = 2;
}

message CreateUserResponse {
  int32 id = 1;
}

service User {
  // CreateUser 创建用户
  rpc CreateUser(CreateUserRequest) returns(CreateUserResponse);
}
```

### ❌ Incorrect Patterns

```protobuf
// DON'T: Define messages in separate imported files
syntax = "proto3";

package user;

import "common.proto";  // ❌ Messages should not be imported

option go_package = "./types/user";

service User {
  rpc CreateUser(common.Request) returns(common.Response);  // ❌ Wrong!
}
```

## Directory Structure

```
myproject/
├── contracts/
│   └── proto/                 # Shared source-of-truth protos
├── desc/
│   └── proto/
│       ├── user.proto          # Temporary/local generation input
│       ├── order.proto         # Temporary/local generation input
│       └── version.proto       # Temporary/local generation input copied by hook
└── internal/
    ├── proto/                  # Generated proto code
    │   ├── user/
    │   ├── order/
    │   └── product/
    └── svc/
        └── servicecontext.go   # Auto-registers all proto services
```

## Code Generation

Generate RPC code from proto files:

```bash
# Generate from specific proto file
jzero gen --desc desc/proto/user.proto

# Generate all proto files
jzero gen
```

In `wownow-micro`, the normal contract update flow is:

```bash
# 1. Install or refresh the pinned protobuf toolchain when needed
make proto-setup

# 2. Refresh shared protobuf/grpc Go code
make proto

# 3. If an RPC service implementation changed, regenerate service code
make gen

# 4. Validate BFF/service RPC layout after adapting callers
make verify-rpc
```

Notes:

- `make proto-setup` installs the repository-pinned `protoc`, `protoc-gen-go`, and `protoc-gen-go-grpc` binaries into `.tools/proto/bin/`.
- `make proto` updates `contracts/gen/**`; this is the shared client/type output used by BFFs, services, and tests.
- There is no separate BFF client generation step after a proto change.
- Only update a BFF `internal/rpc/<domain>/clientset.go` when you need to expose a newly added grpc service accessor.
- `make verify-rpc` is a pure layout check; it does not require local protobuf tools to be installed.
- Do not keep `svcCtx.<Domain> != nil && svcCtx.<Domain>.<Accessor>() != nil` style guards around required RPC clientsets.
- Do not recreate `typed/`, `types/`, `model/`, or `desc/proto/` under BFF `internal/rpc/**`.

## Key Features

### Multi-Proto Support

Unlike goctl, jzero supports multiple proto files and automatically:
- Scans `desc/proto/` directory
- Generates code for all proto files
- Registers all services to zrpc server

For `wownow-micro`, that means hooks must prepare `desc/proto/` before generation if the canonical source lives under `contracts/proto/`.

### HTTP Gateway Support

Proto files can define HTTP mappings for REST endpoints:

```protobuf
service User {
  // CreateUser 创建用户
  rpc CreateUser(CreateUserRequest) returns(CreateUserResponse) {
    option (google.api.http) = {
      post: "/api/v1/user/create"
      body: "*"
    };
  };

  // GetUser 获取用户信息
  rpc GetUser(GetUserRequest) returns(GetUserResponse) {
    option (google.api.http) = {
      get: "/api/v1/user/{id}"
    };
  };
}
```

### OpenAPI Documentation

Generate OpenAPI/Swagger documentation:

```protobuf
import "grpc-gateway/protoc-gen-openapiv2/options/annotations.proto";

option (grpc.gateway.protoc_gen_openapiv2.options.openapiv2_swagger) = {
  info: {
    title: "User Service"
    version: "1.0"
    description: "User management service"
  }
};
```

## Related Topics

- [Proto Field Validation](proto-validation.md) - Adding validation to proto messages
- [Proto Middleware](proto-middleware.md) - Using middleware with proto services
- [jzero Documentation](https://docs.jzero.io) - Official documentation
