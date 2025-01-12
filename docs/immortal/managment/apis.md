---
sidebar_position: 3
---

# Management APIs

Here is a list of APIs between manager and immortals. All of these APIs are through [gRPC](https://grpc.io/) and [proto files](https://protobuf.dev/overview/).


## Relay to Manager

Here is a list of APIs which is provided by manager and relay calls them:

###  RegisterService

The relays call this endpoint to receive an ID (a unique id between all relays in the same system for manager to identify them). 

Here is an example of register request:

```proto
// Request to register a service.
message RegisterServiceRequest {
  string port = 1;                        // Service grpc port.
  uint32 heartbeat_duration_in_sec = 2;   // Heartbeat interval in seconds for calling Status endpoint of relay.
  ServiceTypeEnum type = 3;               // Type of the service. (e.g., RELAY)
  string region = 4;                      // Region of the service.
}
```

Response example:

```proto
// Response after registering a service.
message RegisterServiceResponse {
  bool success = 1;                      // Indicates if registration was successful.
  optional string message = 2;           // Optional message with additional information.
  string token = 3;                      // Token assigned to the registered service.
}
```

> Relay will send the id in the header of `x-identifier` in each request to manager. this helps the manager to know who is talking with and take different actions for same request but different relays.

### GetParameters

Relays will call this endpoint to get the parameters config from manager which requires the `x-identifier` header as well.

Request example:

```proto
// Empty request used for methods that do not require parameters.
message GetParametersRequest {}
```

Response example:

```proto
// Response containing configuration details.
message GetParametersResponse {
  limitations limitations = 1;           // Configuration limitations.
  string url = 2;                        // Relay public URL.
}
```

Types used:

```proto
// Configuration limitations.
message limitations {
  int32 max_message_length = 1;          // Maximum allowed message length.
  int32 max_subscriptions = 2;           // Maximum number of subscriptions allowed.
  int32 max_subid_length = 4;            // Maximum length of a subscription ID.
  int32 min_pow_difficulty = 5;          // Minimum proof-of-work difficulty.
  bool auth_required = 6;                // Indicates if authentication is required.
  bool payment_required = 7;             // Indicates if payment is required.
  bool restricted_writes = 8;            // Indicates if writes are restricted.
  int32 max_event_tags = 9;              // Maximum number of event tags allowed.
  int32 max_content_length = 10;         // Maximum length of content allowed.
  int64 created_at_lower_limit = 11;     // Lower limit for creation timestamps.
  int64 created_at_upper_limit = 12;     // Upper limit for creation timestamps.
  uint32 default_query_limit = 13;       // Default of query limit.
  uint32 max_query_limit = 14;           // Maximum of query limit.
}


// Types of services that can be registered.
enum ServiceTypeEnum {
  UNKNOWN = 0;                           // Unknown service type.
  RELAY = 1;                             // Relay service type.
}
```

### AddLog

Relays may send system level logs to manager.

```proto
message AddLogRequest {
  string message = 1;                   
  string stack = 2;                      
}
```

Response example:

```proto
message AddLogResponse {
  bool success = 1;               
  optional string message = 2;         
}
```

## Manager to Relay

Here is a list of APIs provided by relays which would be called by managers:

### Status

This API will return the health status of the immortal instance beside status of services the relay use such as database and more.

Request example:

```proto
message StatusRequest {}
```

Response example:

```proto
message StatusResponse {
    repeated Service services = 1;
    int64 uptime = 2;
    string version = 3;
}
```

Types used:

```proto
enum Status {
    UNKNOWN = 0;
    CONNECTED = 1;
    DISCONNECTED = 2;
}

message Service {
    string name = 1;
    Status status = 2;
    string message = 3;
}
```

The manager can show the response in a monitoring system, make notifications, shutdown the relays with specific kind of issues and more.

### SetConfig

> TODO.

### Shutdown

Manager can send a shutdown signal to each relay independently.

```proto
message ShutdownRequest {}
```

Response example:

```proto
message ShutdownResponse {}
```
