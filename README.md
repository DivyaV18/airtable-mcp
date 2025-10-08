# Airtable MCP Server

A comprehensive Model Context Protocol (MCP) server for Airtable integration, providing tools to interact with Airtable bases, tables, records, and comments.

## 🚀 Features

### Available Tools

| Tool | Description | Status |
|------|-------------|--------|
| `AIRTABLE_CREATE_BASE` | Create base configuration (simulated) | ⚠️ Limited |
| `AIRTABLE_CREATE_COMMENT` | Create comments on records | ✅ Working |
| `AIRTABLE_CREATE_FIELD` | Create fields in tables | ✅ Working |
| `AIRTABLE_CREATE_MULTIPLE_RECORDS` | Create multiple records | ✅ Working |
| `AIRTABLE_CREATE_RECORD` | Create single record | ✅ Working |
| `AIRTABLE_CREATE_TABLE` | Create tables in bases | ✅ Working |
| `AIRTABLE_DELETE_COMMENT` | Delete comments from records | ✅ Working |
| `AIRTABLE_DELETE_MULTIPLE_RECORDS` | Delete multiple records (up to 10) | ✅ Working |
| `AIRTABLE_DELETE_RECORD` | Delete single record | ✅ Working |
| `AIRTABLE_GET_BASE_SCHEMA` | Get base schema information | ✅ Working |
| `AIRTABLE_GET_RECORD` | Get specific record | ✅ Working |
| `AIRTABLE_GET_USER_INFO` | Get authenticated user info | ✅ Working |
| `AIRTABLE_LIST_BASES` | List accessible bases | ✅ Working |
| `AIRTABLE_LIST_COMMENTS` | List comments for a record | ✅ Working |
| `AIRTABLE_LIST_RECORDS` | List records with filtering | ✅ Working |
| `AIRTABLE_UPDATE_MULTIPLE_RECORDS` | Update multiple records | ✅ Working |
| `AIRTABLE_UPDATE_RECORD` | Update single record | ✅ Working |

## 📋 Prerequisites

- Python 3.8 or higher
- Airtable account with API access
- Valid Airtable API key

## 🛠️ Installation

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd airtable-mcp
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up environment variables:**
   ```bash
   cp env.example .env
   ```

4. **Configure your `.env` file:**
   ```env
   # Airtable API Configuration
   AIRTABLE_API_KEY=your_airtable_api_key_here

   # Optional: Base ID for testing (if you have an existing base)
   AIRTABLE_BASE_ID=your_base_id_here
   ```

## 🔑 Getting Your Airtable API Key

1. Go to [Airtable Account](https://airtable.com/account)
2. Navigate to "Personal access tokens"
3. Click "Create new token"
4. Give it a name and select scopes:
   - `data.records:read`
   - `data.records:write`
   - `schema.bases:read`
   - `schema.bases:write`
   - `webhook:manage`
5. Copy the generated token to your `.env` file

## 🚀 Usage

### Running the Server

1. **Start the MCP server:**
   ```bash
   python airtable_mcp_server.py
   ```

2. **Using MCP Inspector:**
   - Install MCP Inspector: `npm install -g @modelcontextprotocol/inspector`
   - Run: `mcp-inspector`
   - Configure your `mcp-config.json`:
   ```json
   {
     "mcpServers": {
       "airtable": {
         "command": "python",
         "args": ["airtable_mcp_server.py"],
         "cwd": "D:/clg files/PROJECTS/airtable-mcp",
         "env": {
           "AIRTABLE_API_KEY": "your_airtable_api_key_here"
         }
       }
     }
   }
   ```

### Example Usage

#### Create a Record
```json
{
  "base_id": "appnJFyljOHYOYSji",
  "table_id_or_name": "Simple Table",
  "fields": "{\"Name\": \"New Task\", \"Status\": \"Pending\"}"
}
```

#### List Records
```json
{
  "base_id": "appnJFyljOHYOYSji",
  "table_id_or_name": "Simple Table",
  "max_records": 10
}
```

#### Update a Record
```json
{
  "base_id": "appnJFyljOHYOYSji",
  "table_id_or_name": "Simple Table",
  "record_id": "recXXXXXXXXXXXXXX",
  "fields": "{\"Status\": \"Completed\"}"
}
```

## 📚 Tool Documentation

### AIRTABLE_CREATE_BASE
⚠️ **Important**: This tool only validates configuration and returns a simulated response. Airtable API does not support programmatic base creation.

**Parameters:**
- `name` (str): Name of the base
- `tables` (str): JSON string with table configuration
- `workspace_id` (str): Workspace ID

**Example:**
```json
{
  "name": "My Project Base",
  "tables": "[{\"name\": \"Tasks\", \"fields\": [{\"name\": \"Task Name\", \"type\": \"singleLineText\"}]}]",
  "workspace_id": "wspXXXXXXXXXXXXXX"
}
```

### AIRTABLE_CREATE_RECORD
Creates a new record in a table.

**Parameters:**
- `base_id` (str): Base ID
- `table_id_or_name` (str): Table ID or name
- `fields` (str): JSON string with field values

**Example:**
```json
{
  "base_id": "appnJFyljOHYOYSji",
  "table_id_or_name": "Simple Table",
  "fields": "{\"Name\": \"New Task\", \"Status\": \"Pending\"}"
}
```

### AIRTABLE_LIST_RECORDS
Lists records with filtering and pagination options.

**Parameters:**
- `base_id` (str): Base ID
- `table_id_or_name` (str): Table ID or name
- `max_records` (int): Maximum records to return (default: 0 = no limit)
- `filter_by_formula` (str): Filter formula
- `sort` (str): JSON string with sort configuration

**Example:**
```json
{
  "base_id": "appnJFyljOHYOYSji",
  "table_id_or_name": "Simple Table",
  "max_records": 5,
  "filter_by_formula": "{Status} = 'Pending'"
}
```

## 🔧 Configuration

### MCP Inspector Configuration

Create `mcp-config.json`:
```json
{
  "mcpServers": {
    "airtable": {
      "command": "python",
      "args": ["airtable_mcp_server.py"],
      "cwd": "D:/clg files/PROJECTS/airtable-mcp",
      "env": {
        "AIRTABLE_API_KEY": "your_airtable_api_key_here"
      }
    }
  }
}
```

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `AIRTABLE_API_KEY` | Your Airtable API key | Yes |
| `AIRTABLE_BASE_ID` | Default base ID for testing | No |

## 🚨 Important Limitations

### AIRTABLE_CREATE_BASE Limitation
- **Cannot create real bases** - Airtable API doesn't support this
- **Returns simulated response** - Only validates configuration
- **Manual creation required** - Use Airtable web interface

### API Rate Limits
- **5 requests per second** per base
- **Rate limiting** is handled automatically
- **429 errors** will be retried with backoff

### Record Limits
- **Delete multiple records**: Maximum 10 records per request
- **Create multiple records**: No specific limit, but consider performance
- **List records**: Pagination supported with `offset` parameter

## 🛠️ Troubleshooting

### Common Issues

1. **Authentication Error**
   ```
   Airtable API Error: AUTHENTICATION_REQUIRED
   ```
   - Check your `AIRTABLE_API_KEY` in `.env` file
   - Verify the API key has correct scopes

2. **Base Not Found**
   ```
   Airtable API Error: INVALID_PERMISSIONS_OR_MODEL_NOT_FOUND
   ```
   - Verify the base ID is correct
   - Check if you have access to the base

3. **Request Timeout**
   ```
   MCP error -32001: Request timed out
   ```
   - Check your internet connection
   - Verify Airtable API is accessible

### Debug Mode

Enable debug logging by setting environment variable:
```bash
export AIRTABLE_DEBUG=true
```

## 📝 Field Types Reference

| Type | Description | Options Required |
|------|-------------|------------------|
| `singleLineText` | Single line text | No |
| `multilineText` | Multi-line text | No |
| `singleSelect` | Single choice | Yes (choices) |
| `multipleSelects` | Multiple choices | Yes (choices) |
| `number` | Number | No |
| `date` | Date | No |
| `checkbox` | True/False | No |
| `email` | Email address | No |
| `url` | URL | No |

