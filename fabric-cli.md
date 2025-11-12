# Microsoft Fabric CLI Cheat Sheet

## Installation and Setup

```bash
# Install Microsoft Fabric CLI (via Azure CLI extension)
az extension add --name fabric

# Update to latest version
az extension update --name fabric

# Check version
az extension show --name fabric
```

## Authentication

```bash
# Login to Azure
az login

# Login with specific tenant
az login --tenant <tenant-id>

# Login with service principal
az login --service-principal -u <app-id> -p <password> --tenant <tenant-id>

# Set default subscription
az account set --subscription <subscription-id>

# Show current account
az account show
```

## Workspace Management

### List Workspaces

```bash
# List all workspaces
az fabric workspace list

# List workspaces in specific capacity
az fabric workspace list --capacity <capacity-name>

# List with table output
az fabric workspace list --output table
```

### Create and Manage Workspaces

```bash
# Create workspace
az fabric workspace create --name <workspace-name> --capacity <capacity-name>

# Get workspace details
az fabric workspace show --name <workspace-name>

# Update workspace
az fabric workspace update --name <workspace-name> --description "New description"

# Delete workspace
az fabric workspace delete --name <workspace-name>
```

## Capacity Management

```bash
# List capacities
az fabric capacity list

# Get capacity details
az fabric capacity show --name <capacity-name>

# Create capacity
az fabric capacity create --name <capacity-name> --sku <sku-name> --admin <admin-email>

# Update capacity
az fabric capacity update --name <capacity-name> --sku <new-sku>

# Delete capacity
az fabric capacity delete --name <capacity-name>

# Suspend capacity
az fabric capacity suspend --name <capacity-name>

# Resume capacity
az fabric capacity resume --name <capacity-name>
```

## Lakehouse Operations

### Manage Lakehouses

```bash
# List lakehouses in workspace
az fabric lakehouse list --workspace <workspace-name>

# Create lakehouse
az fabric lakehouse create --name <lakehouse-name> --workspace <workspace-name>

# Get lakehouse details
az fabric lakehouse show --name <lakehouse-name> --workspace <workspace-name>

# Delete lakehouse
az fabric lakehouse delete --name <lakehouse-name> --workspace <workspace-name>
```

### Lakehouse Files

```bash
# List files in lakehouse
az fabric lakehouse files list --lakehouse <lakehouse-name> --workspace <workspace-name>

# Upload file to lakehouse
az fabric lakehouse files upload --lakehouse <lakehouse-name> --workspace <workspace-name> --file <local-file> --path <destination-path>

# Download file from lakehouse
az fabric lakehouse files download --lakehouse <lakehouse-name> --workspace <workspace-name> --path <source-path> --file <local-file>

# Delete file from lakehouse
az fabric lakehouse files delete --lakehouse <lakehouse-name> --workspace <workspace-name> --path <file-path>
```

## Data Pipeline Operations

```bash
# List pipelines
az fabric pipeline list --workspace <workspace-name>

# Create pipeline
az fabric pipeline create --name <pipeline-name> --workspace <workspace-name> --definition @pipeline.json

# Get pipeline details
az fabric pipeline show --name <pipeline-name> --workspace <workspace-name>

# Update pipeline
az fabric pipeline update --name <pipeline-name> --workspace <workspace-name> --definition @updated-pipeline.json

# Delete pipeline
az fabric pipeline delete --name <pipeline-name> --workspace <workspace-name>

# Run pipeline
az fabric pipeline run create --pipeline <pipeline-name> --workspace <workspace-name>

# Get pipeline run status
az fabric pipeline run show --run-id <run-id> --workspace <workspace-name>

# List pipeline runs
az fabric pipeline run list --pipeline <pipeline-name> --workspace <workspace-name>
```

## Notebook Management

```bash
# List notebooks
az fabric notebook list --workspace <workspace-name>

# Create notebook
az fabric notebook create --name <notebook-name> --workspace <workspace-name>

# Import notebook
az fabric notebook import --name <notebook-name> --workspace <workspace-name> --file <notebook-file>

# Export notebook
az fabric notebook export --name <notebook-name> --workspace <workspace-name> --file <output-file>

# Delete notebook
az fabric notebook delete --name <notebook-name> --workspace <workspace-name>
```

## Semantic Model (Dataset) Operations

```bash
# List semantic models
az fabric semanticmodel list --workspace <workspace-name>

# Get semantic model details
az fabric semanticmodel show --name <model-name> --workspace <workspace-name>

# Refresh semantic model
az fabric semanticmodel refresh --name <model-name> --workspace <workspace-name>

# Get refresh history
az fabric semanticmodel refresh list --name <model-name> --workspace <workspace-name>

# Cancel refresh
az fabric semanticmodel refresh cancel --refresh-id <refresh-id> --name <model-name> --workspace <workspace-name>
```

## Report Operations

```bash
# List reports
az fabric report list --workspace <workspace-name>

# Get report details
az fabric report show --name <report-name> --workspace <workspace-name>

# Clone report
az fabric report clone --name <report-name> --workspace <workspace-name> --target-workspace <target-workspace>

# Export report
az fabric report export --name <report-name> --workspace <workspace-name> --format PDF --file report.pdf

# Delete report
az fabric report delete --name <report-name> --workspace <workspace-name>
```

## Data Warehouse Operations

```bash
# List warehouses
az fabric warehouse list --workspace <workspace-name>

# Create warehouse
az fabric warehouse create --name <warehouse-name> --workspace <workspace-name>

# Get warehouse details
az fabric warehouse show --name <warehouse-name> --workspace <workspace-name>

# Delete warehouse
az fabric warehouse delete --name <warehouse-name> --workspace <workspace-name>

# Get connection string
az fabric warehouse connection-string --name <warehouse-name> --workspace <workspace-name>
```

## KQL Database Operations

```bash
# List KQL databases
az fabric kql-database list --workspace <workspace-name>

# Create KQL database
az fabric kql-database create --name <database-name> --workspace <workspace-name>

# Get KQL database details
az fabric kql-database show --name <database-name> --workspace <workspace-name>

# Delete KQL database
az fabric kql-database delete --name <database-name> --workspace <workspace-name>
```

## Access Control and Permissions

```bash
# List workspace users
az fabric workspace user list --workspace <workspace-name>

# Add user to workspace
az fabric workspace user add --workspace <workspace-name> --email <user-email> --role <role>

# Update user role
az fabric workspace user update --workspace <workspace-name> --email <user-email> --role <new-role>

# Remove user from workspace
az fabric workspace user remove --workspace <workspace-name> --email <user-email>

# Roles: Admin, Member, Contributor, Viewer
```

## Monitoring and Logs

```bash
# Get workspace activity
az fabric workspace activity list --workspace <workspace-name>

# Get workspace usage metrics
az fabric workspace metrics --workspace <workspace-name>

# Get capacity metrics
az fabric capacity metrics --name <capacity-name>
```

## Git Integration

```bash
# Connect workspace to Git
az fabric workspace git connect --workspace <workspace-name> --git-provider <provider> --repository <repo-url> --branch <branch-name>

# Sync with Git
az fabric workspace git sync --workspace <workspace-name>

# Get Git status
az fabric workspace git status --workspace <workspace-name>

# Disconnect from Git
az fabric workspace git disconnect --workspace <workspace-name>

# Commit changes to Git
az fabric workspace git commit --workspace <workspace-name> --message "Commit message"
```

## Deployment Pipelines

```bash
# List deployment pipelines
az fabric deployment-pipeline list

# Create deployment pipeline
az fabric deployment-pipeline create --name <pipeline-name>

# Get pipeline details
az fabric deployment-pipeline show --name <pipeline-name>

# Add stage to pipeline
az fabric deployment-pipeline stage add --pipeline <pipeline-name> --workspace <workspace-name> --stage-order <order>

# Deploy to next stage
az fabric deployment-pipeline deploy --pipeline <pipeline-name> --source-stage <stage> --target-stage <stage>

# Delete deployment pipeline
az fabric deployment-pipeline delete --name <pipeline-name>
```

## Common JSON Configuration

### Sample pipeline.json

```json
{
  "properties": {
    "activities": [
      {
        "name": "CopyData",
        "type": "Copy",
        "inputs": [
          {
            "referenceName": "SourceDataset"
          }
        ],
        "outputs": [
          {
            "referenceName": "SinkDataset"
          }
        ]
      }
    ]
  }
}
```

## Useful Options

```bash
# Output formats
--output table        # Human-readable table
--output json         # JSON format
--output jsonc        # Colorized JSON
--output yaml         # YAML format
--output tsv          # Tab-separated values

# Query results with JMESPath
--query "property.value"

# Verbose output
--verbose

# Debug mode
--debug
```

## Automation and Scripting

```bash
# Use in scripts with error handling
if az fabric workspace show --name myworkspace &> /dev/null; then
    echo "Workspace exists"
else
    az fabric workspace create --name myworkspace --capacity mycapacity
fi

# Loop through workspaces
for workspace in $(az fabric workspace list --query "[].name" -o tsv); do
    echo "Processing $workspace"
    az fabric lakehouse list --workspace $workspace
done

# Export to variable
WORKSPACE_ID=$(az fabric workspace show --name myworkspace --query id -o tsv)
```

## Best Practices

1. **Use Resource Groups**: Organize Fabric resources within Azure resource groups
2. **Set Defaults**: Use `az configure` to set default workspace or capacity
3. **Service Principals**: Use service principals for automation instead of user accounts
4. **Error Handling**: Always check exit codes in scripts
5. **Version Control**: Store pipeline and configuration JSON files in version control
6. **Least Privilege**: Grant minimum necessary permissions to users and service principals

## Troubleshooting

```bash
# Enable debug logging
az fabric workspace list --debug

# Check extension version
az extension show --name fabric

# Verify authentication
az account show

# Test connectivity
az fabric capacity list
```

## Environment Variables

```bash
# Set default output format
export AZURE_CLI_OUTPUT=table

# Disable telemetry
export AZURE_CORE_COLLECT_TELEMETRY=no

# Set default subscription
export AZURE_SUBSCRIPTION_ID=<subscription-id>
```
