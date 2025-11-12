# Databricks CLI Cheat Sheet

## Installation and Setup

```bash
# Install Databricks CLI
pip install databricks-cli

# Configure authentication
databricks configure --token

# Test connection
databricks workspace ls /
```

## Authentication

```bash
# Configure with token (interactive)
databricks configure --token

# Configure with profile
databricks configure --token --profile production

# Set host and token via environment variables
export DATABRICKS_HOST="https://your-workspace.cloud.databricks.com"
export DATABRICKS_TOKEN="your-token"

# Use specific profile
databricks --profile production workspace ls /
```

## Workspace Management

### List and Navigate

```bash
# List workspace items
databricks workspace ls /

# List recursively
databricks workspace ls -l /Users

# List with long format
databricks workspace ls -l /
```

### Import and Export

```bash
# Import notebook
databricks workspace import notebook.py /Users/user@example.com/notebook

# Import with format
databricks workspace import -f PYTHON -l PYTHON notebook.py /path/to/notebook

# Export notebook
databricks workspace export /Users/user@example.com/notebook -f SOURCE

# Export as HTML
databricks workspace export /Users/user@example.com/notebook -f HTML -o output.html

# Export directory recursively
databricks workspace export_dir /Users/user@example.com/folder ./local_folder
```

### Create and Delete

```bash
# Create directory
databricks workspace mkdirs /Users/user@example.com/newfolder

# Delete item
databricks workspace delete /Users/user@example.com/notebook

# Delete directory recursively
databricks workspace delete -r /Users/user@example.com/folder
```

## Cluster Management

### List Clusters

```bash
# List all clusters
databricks clusters list

# List with JSON output
databricks clusters list --output JSON
```

### Create Cluster

```bash
# Create cluster from JSON config
databricks clusters create --json-file cluster_config.json

# Create cluster from JSON string
databricks clusters create --json '{
  "cluster_name": "my-cluster",
  "spark_version": "11.3.x-scala2.12",
  "node_type_id": "i3.xlarge",
  "num_workers": 2
}'
```

### Cluster Operations

```bash
# Start cluster
databricks clusters start --cluster-id <cluster-id>

# Stop cluster
databricks clusters stop --cluster-id <cluster-id>

# Restart cluster
databricks clusters restart --cluster-id <cluster-id>

# Delete cluster
databricks clusters delete --cluster-id <cluster-id>

# Get cluster info
databricks clusters get --cluster-id <cluster-id>

# Edit cluster
databricks clusters edit --json-file updated_config.json
```

## Jobs Management

### List Jobs

```bash
# List all jobs
databricks jobs list

# List with JSON output
databricks jobs list --output JSON
```

### Create and Manage Jobs

```bash
# Create job
databricks jobs create --json-file job_config.json

# Get job details
databricks jobs get --job-id <job-id>

# Delete job
databricks jobs delete --job-id <job-id>

# Update job
databricks jobs reset --job-id <job-id> --json-file updated_job_config.json
```

### Run Jobs

```bash
# Run job now
databricks jobs run-now --job-id <job-id>

# Run with parameters
databricks jobs run-now --job-id <job-id> --python-params '["param1", "param2"]'

# Run with notebook parameters
databricks jobs run-now --job-id <job-id> --notebook-params '{"key": "value"}'

# List job runs
databricks jobs runs list --job-id <job-id>

# Get run output
databricks jobs runs get-output --run-id <run-id>

# Cancel run
databricks jobs runs cancel --run-id <run-id>
```

## DBFS (Databricks File System)

### List and Navigate

```bash
# List DBFS root
databricks fs ls dbfs:/

# List with long format
databricks fs ls -l dbfs:/FileStore

# List recursively
databricks fs ls -r dbfs:/mnt
```

### File Operations

```bash
# Copy file to DBFS
databricks fs cp local_file.txt dbfs:/FileStore/

# Copy from DBFS to local
databricks fs cp dbfs:/FileStore/file.txt ./local_file.txt

# Copy recursively
databricks fs cp -r ./local_folder dbfs:/FileStore/folder

# Remove file
databricks fs rm dbfs:/FileStore/file.txt

# Remove directory recursively
databricks fs rm -r dbfs:/FileStore/folder

# Create directory
databricks fs mkdirs dbfs:/FileStore/newfolder

# Move/rename file
databricks fs mv dbfs:/FileStore/old.txt dbfs:/FileStore/new.txt
```

### Read and Write

```bash
# Read file content
databricks fs cat dbfs:/FileStore/file.txt

# Write to file (from stdin)
echo "content" | databricks fs put dbfs:/FileStore/file.txt
```

## Secrets Management

### Scopes

```bash
# Create secret scope
databricks secrets create-scope --scope my-scope

# List secret scopes
databricks secrets list-scopes

# Delete secret scope
databricks secrets delete-scope --scope my-scope
```

### Secrets

```bash
# Put secret
databricks secrets put --scope my-scope --key my-key

# List secrets in scope
databricks secrets list --scope my-scope

# Delete secret
databricks secrets delete --scope my-scope --key my-key
```

### ACLs (Access Control Lists)

```bash
# Put ACL
databricks secrets put-acl --scope my-scope --principal user@example.com --permission READ

# List ACLs
databricks secrets list-acls --scope my-scope

# Get ACL
databricks secrets get-acl --scope my-scope --principal user@example.com

# Delete ACL
databricks secrets delete-acl --scope my-scope --principal user@example.com
```

## Libraries

```bash
# List cluster libraries
databricks libraries list --cluster-id <cluster-id>

# Install library
databricks libraries install --cluster-id <cluster-id> --pypi-package pandas

# Install from Maven
databricks libraries install --cluster-id <cluster-id> --maven-coordinates org.apache.spark:spark-avro_2.12:3.0.0

# Uninstall library
databricks libraries uninstall --cluster-id <cluster-id> --pypi-package pandas
```

## Instance Pools

```bash
# List instance pools
databricks instance-pools list

# Create instance pool
databricks instance-pools create --json-file pool_config.json

# Get instance pool
databricks instance-pools get --instance-pool-id <pool-id>

# Delete instance pool
databricks instance-pools delete --instance-pool-id <pool-id>
```

## Groups and Users

```bash
# List groups
databricks groups list

# Create group
databricks groups create --group-name developers

# Add member to group
databricks groups add-member --parent-name developers --user-name user@example.com

# Remove member from group
databricks groups remove-member --parent-name developers --user-name user@example.com

# List group members
databricks groups list-members --group-name developers
```

## Tokens

```bash
# Create token
databricks tokens create --comment "My automation token" --lifetime-seconds 3600

# List tokens
databricks tokens list

# Revoke token
databricks tokens revoke --token-id <token-id>
```

## Common Configuration

### Sample cluster_config.json

```json
{
  "cluster_name": "my-cluster",
  "spark_version": "11.3.x-scala2.12",
  "node_type_id": "i3.xlarge",
  "num_workers": 2,
  "autoscale": {
    "min_workers": 1,
    "max_workers": 5
  },
  "spark_conf": {
    "spark.speculation": "true"
  },
  "aws_attributes": {
    "availability": "SPOT",
    "zone_id": "us-west-2a"
  }
}
```

### Sample job_config.json

```json
{
  "name": "my-job",
  "tasks": [
    {
      "task_key": "notebook_task",
      "notebook_task": {
        "notebook_path": "/Users/user@example.com/notebook",
        "base_parameters": {
          "param1": "value1"
        }
      },
      "existing_cluster_id": "cluster-id"
    }
  ],
  "schedule": {
    "quartz_cron_expression": "0 0 12 * * ?",
    "timezone_id": "UTC"
  }
}
```

## Useful Tips

```bash
# Get CLI version
databricks --version

# Use JSON output for scripting
databricks clusters list --output JSON | jq '.clusters[] | .cluster_id'

# Debug mode
databricks --debug workspace ls /

# Use different profile
databricks --profile prod clusters list
```

## API Endpoints Reference

The Databricks CLI is a wrapper around the Databricks REST API. Common endpoint patterns:

- `/api/2.0/clusters/*` - Cluster operations
- `/api/2.0/jobs/*` - Jobs operations
- `/api/2.0/workspace/*` - Workspace operations
- `/api/2.0/dbfs/*` - DBFS operations
- `/api/2.0/secrets/*` - Secrets operations
