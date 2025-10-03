# MCP Protocol IoT Control Usage Instructions

> This document explains how to implement IoT control for ESP32 devices based on the MCP protocol. For detailed protocol flow, please refer to [`mcp-protocol.md`](./mcp-protocol.md).

## Introduction

MCP (Model Context Protocol) is the new generation protocol recommended for IoT control. It uses standard JSON-RPC 2.0 format to discover and call "tools" between the backend and devices, enabling flexible device control.

## Typical Usage Flow

1. After device startup, establish connection with backend through underlying protocol (such as WebSocket/MQTT).
2. Backend initializes session through MCP protocol's `initialize` method.
3. Backend obtains all tools (functions) supported by the device and their parameter descriptions through `tools/list`.
4. Backend calls specific tools through `tools/call` to achieve device control.

For detailed protocol format and interaction, see [`mcp-protocol.md`](./mcp-protocol.md).

## Device-Side Tool Registration Method Description

Devices register callable "tools" through the `McpServer::AddTool` method. Its common function signature is as follows:

```cpp
void AddTool(
    const std::string& name,           // Tool name, recommended to be unique and hierarchical, e.g., self.dog.forward
    const std::string& description,    // Tool description, concise explanation of function, easy for large models to understand
    const PropertyList& properties,    // Input parameter list (can be empty), supports types: boolean, integer, string
    std::function<ReturnValue(const PropertyList&)> callback // Callback implementation when tool is called
);
```
- name: Unique tool identifier, recommended to use "module.function" naming style.
- description: Natural language description, easy for AI/users to understand.
- properties: Parameter list, supports types: boolean, integer, string, can specify ranges and default values.
- callback: Actual execution logic when receiving call request, return value can be bool/int/string.

## Typical Registration Example (Using ESP-Hi as Example)

```cpp
void InitializeTools() {
    auto& mcp_server = McpServer::GetInstance();
    // Example 1: No parameters, control robot forward movement
    mcp_server.AddTool("self.dog.forward", "Robot moves forward", PropertyList(), [this](const PropertyList&) -> ReturnValue {
        servo_dog_ctrl_send(DOG_STATE_FORWARD, NULL);
        return true;
    });
    // Example 2: With parameters, set RGB light color
    mcp_server.AddTool("self.light.set_rgb", "Set RGB color", PropertyList({
        Property("r", kPropertyTypeInteger, 0, 255),
        Property("g", kPropertyTypeInteger, 0, 255),
        Property("b", kPropertyTypeInteger, 0, 255)
    }), [this](const PropertyList& properties) -> ReturnValue {
        int r = properties["r"].value<int>();
        int g = properties["g"].value<int>();
        int b = properties["b"].value<int>();
        led_on_ = true;
        SetLedColor(r, g, b);
        return true;
    });
}
```

## Common Tool Call JSON-RPC Examples

### 1. Get Tool List
```json
{
  "jsonrpc": "2.0",
  "method": "tools/list",
  "params": { "cursor": "" },
  "id": 1
}
```

### 2. Control Chassis Forward Movement
```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "self.chassis.go_forward",
    "arguments": {}
  },
  "id": 2
}
```

### 3. Switch Light Mode
```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "self.chassis.switch_light_mode",
    "arguments": { "light_mode": 3 }
  },
  "id": 3
}
```

### 4. Camera Flip
```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "self.camera.set_camera_flipped",
    "arguments": {}
  },
  "id": 4
}
```

## Notes
- Tool names, parameters, and return values should be based on device-side `AddTool` registration.
- All new projects are recommended to uniformly adopt MCP protocol for IoT control.
- For detailed protocol and advanced usage, please refer to [`mcp-protocol.md`](./mcp-protocol.md).
