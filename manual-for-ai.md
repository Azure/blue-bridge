# BlueBridge MCP Tools Manual for AI

This document describes the MCP tools provided by the BlueBridge server. These tools are intended for use by AI agents like Cline.

## Azure Resource Graph Tool (`blue_bridge_query_azure_resource_graph`)

**Description:** Execute Azure Resource Graph (ARG) query.

**Parameters:**
- `argQuery` (string, required): The Azure Resource Graph query to execute.

**Example Prompt (List Azure Subscriptions):**
```
Use the `blue_bridge_query_azure_resource_graph` mcp function the run the below Azure Resource Graph query for listing all the azure subscriptions.

ResourceContainers
| where type =~ 'microsoft.resources/subscriptions'
| project subscriptionId, subscriptionName = name, tags
```

**Example Prompt (List Virtual Machines):**
```
Use the `blue_bridge_query_azure_resource_graph` mcp function to run the below Azure Resource Graph query for listing all virtual machines.

Resources
| where type =~ 'microsoft.compute/virtualmachines'
| project name, location, resourceGroup, subscriptionId
```

**Example Prompt (Count Resources by Type):**
```
Use the `blue_bridge_query_azure_resource_graph` mcp function to run the below Azure Resource Graph query to count resources by type.

Resources
| summarize count() by type
```

**Example Prompt (Identify Underutilized VMs that can be turned off or shrink):**
```
Use the `blue_bridge_query_azure_resource_graph` mcp function to run the below Azure Resource Graph query to identify VMs with "Right-size or shutdown underutilized virtual machines" recommendations.

advisorresources
| where type =~ 'microsoft.advisor/recommendations'
| where (properties.category == 'Security' and properties.lastUpdated > ago(60h)) or properties.lastUpdated >= ago(1d)
| where isempty(properties.tracked) or properties.tracked == false
| extend problem = tostring(properties.shortDescription.problem)
| where problem == "Right-size or shutdown underutilized virtual machines"
```

## Grafana Tool (`blue_bridge_grafana_query_data_source`)

**Description:** Query Grafana data source.

**Parameters:**
- `query` (string, required): The Grafana data source query payload.

## Kusto Tool (`blue_bridge_query_kusto`)

**Description:** Execute Azure Data Explorer (Kusto) query.

**Parameters:**
- `databaseName` (string, required): The database name of the Azure Data Explorer (Kusto) cluster.
- `query` (string, required): Azure Data Explorer (Kusto) cluster query.

## ARM Proxy Tool (`blue_bridge_arm_http_*`)

This tool provides several functions for interacting with the Azure Resource Manager (ARM) API via HTTP methods.

### GET (`blue_bridge_arm_http_get`)

**Description:** Sends a GET request to the specified Azure Resource Manager API path.
**Parameters:**
- `requestPath` (string, required): The relative path for the ARM API call (e.g., `/subscriptions/.../resourceGroups/...`).
- `apiVersion` (string, required): The api-version for the ARM API call (e.g., `2023-07-01`).

### PUT (`blue_bridge_arm_http_put`)

**Description:** Sends a PUT request to the specified Azure Resource Manager API path.
**Parameters:**
- `requestPath` (string, required): The relative path for the ARM API call.
- `apiVersion` (string, required): The api-version for the ARM API call.
- `requestBody` (string, required): A JSON string representing the HTTP request body.

### PATCH (`blue_bridge_arm_http_patch`)

**Description:** Sends a PATCH request to the specified Azure Resource Manager API path.
**Parameters:**
- `requestPath` (string, required): The relative path for the ARM API call.
- `apiVersion` (string, required): The api-version for the ARM API call.
- `requestBody` (string, required): A JSON string representing the HTTP request body.

### POST (`blue_bridge_arm_http_post`)

**Description:** Sends a POST request to the specified Azure Resource Manager API path.
**Parameters:**
- `requestPath` (string, required): The relative path for the ARM API call.
- `apiVersion` (string, required): The api-version for the ARM API call.
- `requestBody` (string, optional): A JSON string representing the HTTP request body. Defaults to null/empty.

### DELETE (`blue_bridge_arm_http_delete`)

**Description:** Sends a DELETE request to the specified Azure Resource Manager API path.
**Parameters:**
- `requestPath` (string, required): The relative path for the ARM API call.
- `apiVersion` (string, required): The api-version for the ARM API call.
