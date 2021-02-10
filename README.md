# kps-connector-idl
This repository contains the IDL for KPS Connectors. A KPS Connector is a kubernetes application that implements a
specific GRPC contract. The contract is described in the `connector.proto` file in the `protobuf` directory.
Particularly, the service has to implement three methods in order to fulfil the Connector contract.

```proto
service Connector {
    // GetPayload should return all payloads given a payload kind:
    //   - If payload kind is set to STREAM, it should return all available streams (including discovered streams)
    //   - If payload kind is set to CONFIG, it should return the current config in use.
    rpc GetPayload(GetPayloadRequest) returns (GetPayloadResponse);

    // SetPayload takes a connector ID and a list of payloads and applies those
    // payloads to the relevant connector:
    //   - Payloads of kind STREAM should be subscribed to by the connector
    //   - Payloads of kind CONFIG should be used to update the current connector config
    rpc SetPayload(SetPayloadRequest) returns (SetPayloadResponse);

    // GetEvents returns all the events for a given connector ID. Events can be of type Alert or Status
    rpc GetEvents(GetEventsRequest) returns (GetEventsResponse);
}
```
The `connector.proto` can be used to generate stubs in any language that GRPC supports. A service built in any
language implementing the GRPC interface would qualify as a "Connector" for the KPS ecosystem.
