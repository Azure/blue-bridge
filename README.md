# blue-bridge
A Model Context Protocol (MCP) server implementation for querying and managing Azure resources — built‑in support for Azure Managed Grafana, Azure Data Explorer (Kusto), Azure Resource Graph, Azure Resource Manager and more. Leverages User Identity or Managed Identity for secure, seamless integration via the C# MCP SDK.

## 🛠️ Getting Started

### Run the MCP server in docker
If you want to connect to an Azure Managed Grafana, please set the Grafana endpoint using environment variable `BlueBridgeOptions__AzureManagedGrafanaEndpoint`. If you want to connect to an Azure Data Explorer (Kusto), please set the Kusto Uri using environment variable `BlueBridgeOptions__AzureDataExplorerUri`. You can skip these two if you do not want to use them.

Example command
```
docker run --name bluebridge -p 6688:6688 -e BlueBridgeOptions__AzureManagedGrafanaEndpoint=https://my-azure-managed-grafana-djdfi23fasdagdp.wcus.grafana.azure.com  -e BlueBridgeOptions__AzureDataExplorerUri=https://my-kusto-cluster.westus2.kusto.windows.net bluebridge.azurecr.io/bluebridge:latest
```

```
docker run --name bluebridge -p 6688:6688 -e BlueBridgeOptions__AzureDataExplorerUri=https://my-kusto-cluster.westus2.kusto.windows.net bluebridge.azurecr.io/bluebridge:latest
```

```
docker run --name bluebridge -p 6688:6688 bluebridge.azurecr.io/bluebridge:latest
```

### Login the MCP server.
The MCP server's autehntication is based on azure cli. The container initial log will contain a device login. Please follow the command to login.

```
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FCEZCXCGU to authenticate.
```

After the login, the MCP server will be running on your localhost.

### Connect MCP host
MCP configuration
``` json
{
    "mcpServers": {
      "blue-bridge": {
        "autoApprove": [],
        "disabled": false,
        "timeout": 60,
        "url": "http://localhost:6688/sse",
        "transportType": "sse"
      }
    }
  }
```

### Try it out
You can ask your MCP host with the below prompt:

```
Use the `blue_bridge_query_azure_resource_graph` mcp function the run the below Azure Resource Graph query: resources | limit 2
```
