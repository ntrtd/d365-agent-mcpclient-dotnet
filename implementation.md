# Implementation Plan: d365-agent-mcpclient-dotnet

This document outlines the phased implementation plan for the `d365-agent-mcpclient-dotnet` repository. This library provides a C#/.NET client for interacting with Model Context Protocol (MCP) servers.

## Overall Goal
To develop a robust, well-tested, and easy-to-use C#/.NET MCP client library that enables .NET applications to seamlessly communicate with MCP-compliant servers, such as `d365-agent-mcpserver-dotnet` or `d365-agent-mcpserver-ts`.

---

## Phase 1: Core MCP Client Functionality

*   **Objectives:**
    *   Establish the .NET class library project structure.
    *   Implement core MCP request types (`initialize`, `discoverTools`, `executeTool`).
    *   Handle basic MCP responses and errors.
    *   Publish an initial version of the NuGet package.
*   **Tasks:**
    1.  **Project Setup:**
        *   Initialize a .NET class library project (e.g., targeting .NET Standard 2.0 or a compatible .NET version).
        *   Set up testing framework (e.g., MSTest, NUnit, xUnit).
        *   Configure `csproj` for NuGet packaging (e.g., PackageId `D365.Agent.McpClient.DotNet`).
    2.  **Define Core MCP Types:**
        *   Define C# classes/records for `McpRequest`, `McpResponse`, `McpContentItem`, `McpError`, `McpToolParameter`, `McpToolSchema`, `McpToolDescription`, `McpServerDescription`. These should align with the MCP specification and potentially reuse types from `submodules/csharp-sdk` or a core MCP types NuGet package if available.
    3.  **Implement `McpHttpClient` (or similar) Class:**
        *   Constructor accepting `HttpClient` and `McpClientOptions` (containing `BaseUrl`, default headers, timeout).
        *   `private async Task<McpResponse> SendRequestAsync(McpRequest payload, CancellationToken cancellationToken)`: Internal method for sending JSON-RPC requests.
        *   `public Task<McpResponse> InitializeAsync(object clientInfo, CancellationToken cancellationToken = default)`: Implements the `initialize` handshake.
        *   `public Task<McpServerDescription> DiscoverToolsAsync(CancellationToken cancellationToken = default)`: Implements the `discoverTools` request.
        *   `public Task<McpResponse> ExecuteToolAsync(string toolName, object parameters, CancellationToken cancellationToken = default)`: Implements the `executeTool` request.
    4.  **HTTP Communication:**
        *   Utilize `System.Net.Http.HttpClient` for making requests.
        *   Use `System.Text.Json` for serialization/deserialization.
    5.  **Error Handling:**
        *   Implement robust error handling for network issues (e.g., `HttpRequestException`), timeouts.
        *   Parse and represent MCP error responses from the server (e.g., by throwing specific `McpException` types).
    6.  **Unit Tests:**
        *   Write unit tests for `McpHttpClient` methods, mocking `HttpClient` interactions (e.g., using `Moq` and `HttpClientFactory` patterns or `MockHttpMessageHandler`). Test success and various error cases.
    7.  **CI/CD Pipeline:**
        *   Set up CI (e.g., GitHub Actions) to run tests and build the library.
        *   Set up CD to pack and publish the NuGet package to a feed (e.g., GitHub Packages, NuGet.org, Azure Artifacts) on new versions/tags.
*   **Deliverables:**
    *   Functional `McpHttpClient` capable of basic MCP interactions.
    *   Published NuGet package.

---

## Phase 2: Enhancements and Usability Improvements

*   **Objectives:**
    *   Improve developer experience for consuming MCP tools.
    *   Add support for more advanced MCP features if needed.
*   **Tasks:**
    1.  **Strongly-Typed Tool Execution (Exploration):**
        *   Investigate options for providing more type safety when calling `ExecuteToolAsync`. This could involve:
            *   Generic methods where request and response types can be specified.
            *   Source generators that could create extension methods for `McpHttpClient` based on a downloaded `McpServerDescription` or a local schema definition.
    2.  **Streaming Support (If Applicable):**
        *   If MCP servers in the ecosystem support streaming responses, implement client-side handling for these streams.
    3.  **Advanced Error Handling & Diagnostics:**
        *   Provide more detailed diagnostic information in exceptions or responses.
        *   Allow configuration of logging for client activities.
    4.  **Helper Methods/Extensions:**
        *   Consider adding helper methods for common interaction patterns with MCP servers.
*   **Testing:**
    *   Unit tests for any new helper functions or advanced features.
    *   Integration tests with actual MCP servers (both `.NET` and `TS` versions if available) to verify compatibility.

---

## Phase 3 & 4: Long-term Maintenance & Advanced Features

*   **Objectives:**
    *   Ensure ongoing compatibility and robustness of the client library.
    *   Adapt to MCP protocol evolution.
*   **Tasks:**
    1.  **MCP Protocol Evolution:**
        *   Update the client to support new versions or features of the Model Context Protocol.
    2.  **Performance Optimizations:**
        *   Profile and optimize client performance if specific bottlenecks are identified (e.g., in serialization, HTTP handling).
    3.  **Security Enhancements:**
        *   Review and enhance security aspects, such as the handling of authentication tokens or sensitive data passed through the client.
    4.  **Documentation & Examples:**
        *   Maintain comprehensive API documentation (e.g., using DocFX).
        *   Provide clear usage examples for various scenarios.
    5.  **Bug Fixes & Dependency Updates:**
        *   Address any reported bugs promptly.
        *   Keep .NET and other dependencies up-to-date.
*   **Testing:**
    *   Ongoing regression testing.
    *   Compatibility testing with new versions of MCP servers.

This plan aims to deliver a high-quality .NET MCP client library, facilitating robust communication from .NET applications to any MCP-compliant server within the D365 AI Agent ecosystem and beyond.
