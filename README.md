# Dynamics 365 AI Agent - MCP Client (.NET) (d365-agent-mcpclient-dotnet)

This repository contains the source code for the C#/.NET client library for the Model Context Protocol (MCP). This client enables .NET applications to communicate with MCP-compliant servers.

## Overview

The `d365-agent-mcpclient-dotnet` provides the necessary functionalities for a .NET application to:
*   Discover tools available on an MCP server.
*   Execute those tools with specified parameters.
*   Handle responses and errors according to the MCP specification.

While the primary orchestration layer (`d365-agent-orchestrator`) in the D365 AI Agent system uses the TypeScript MCP client (`d365-agent-mcpclient-ts`), this .NET client can be used by:
*   Other .NET-based backend services or custom applications that need to interact with the `d365-agent-mcpserver-dotnet` or any other MCP server.
*   Testing utilities written in C#.
*   Future .NET-based orchestration components.

## Key Features & Responsibilities

*   **MCP Compliance:** Implements the client-side requirements of the Model Context Protocol.
*   **Tool Discovery:** Provides a method to fetch and parse the `McpServerDescription` from an MCP server.
*   **Tool Execution:** Enables calling specific tools on an MCP server by name, passing structured parameters, and receiving structured responses or errors.
*   **Typed Interactions (Goal):** Aims to provide type safety for tool parameters and responses, potentially through generics or other C# features.
*   **Error Handling:** Manages network errors and interprets MCP error responses from the server.
*   **Extensibility:** Designed to connect to any MCP-compliant server.

## Technology Stack

*   **C# / .NET**
*   `System.Net.Http.HttpClient` for HTTP communication.
*   JSON serialization/deserialization (e.g., `System.Text.Json`).
*   Built to be consumed as a NuGet package (e.g., `D365.Agent.McpClient.DotNet`).
*   May leverage core MCP types and interfaces from `submodules/csharp-sdk` or a corresponding NuGet package for MCP core types.

## Core API (Conceptual)

```csharp
// MCP Client Options
public class McpClientOptions
{
    public Uri BaseUrl { get; set; }
    // Other options like default headers, timeout, etc.
}

// MCP Response (simplified)
public class McpResponse
{
    public bool IsError { get; set; }
    public List<McpContentItem> Content { get; set; }
    // ... other MCP response fields
}

public class McpContentItem 
{
    public string Type { get; set; }
    public string Text { get; set; } // For text content
    public object Data { get; set; } // For json content
    // Other potential content fields
}


// McpHttpClient class
public class McpHttpClient // Or McpClient
{
    public McpHttpClient(HttpClient httpClient, McpClientOptions options);
    public Task<McpServerDescription> DiscoverToolsAsync(CancellationToken cancellationToken = default);
    public Task<McpResponse> ExecuteToolAsync(string toolName, object parameters, CancellationToken cancellationToken = default);
    // Potentially an InitializeAsync method if needed
}
```

## Getting Started

(Details to be added - typically involves installing the NuGet package and instantiating the `McpHttpClient` with the target server URL and an `HttpClient` instance).
Refer to the `implementation.md` in this repository for a detailed development plan.

## Contribution

(Details on contribution guidelines if applicable).
