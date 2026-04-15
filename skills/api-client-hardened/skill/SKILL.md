---
name: postman-hardened
description: API lifecycle management through Postman. Discover collections, run tests, monitor APIs, validate schemas, and publish documentation across the complete API development workflow.
---

# Postman Agent Skill

**Version**: 1.1.0 (Phase 1 - Core API Compatibility)
**API Support**: Postman v10+ (with v9 graceful degradation)

## 🚀 Quick Start

**When you first use this skill, Claude will automatically:**
1. ✅ Validate your API key and connection
2. 📁 Check your workspace configuration
3. 📊 Count your collections and resources
4. 💡 Suggest next steps if setup is incomplete

**First time setup validation:**
```bash
# Claude will run this automatically on first use
python scripts/validate_setup.py
```

**Common first questions that work immediately:**
- "How many collections do I have?" - Lists all collections in your workspace
- "Show me my workspaces" - Displays available workspaces
- "Validate my setup" - Runs comprehensive diagnostics
- "Create a new collection called 'Test API'" - Starts building immediately

**🤖 Note for Claude**: On first use of this skill in a conversation, ALWAYS run:
```bash
python scripts/validate_setup.py
```
This provides immediate diagnostics and context before proceeding with the user's request.

## ✅ Network Compatibility

This skill works across multiple Claude environments with proper proxy configuration.

### Where This Skill Works

| Environment | Status | Notes |
|------------|--------|-------|
| **Claude Web Interface** | ✅ **Fully Supported** | Works with configured proxy |
| **Claude API** (Code Execution) | ✅ **Fully Supported** | No network restrictions |
| **Local Python Scripts** | ✅ **Fully Supported** | Direct execution on your machine |
| **Claude Desktop** | ⚠️ **Limited** | Requires `api.getpostman.com` in network allowlist |

### Proxy Configuration

The skill is designed to work with proxy environments:
- **Keeps proxy environment variables intact** for proper DNS resolution
- **Handles nested HTTP responses** from proxy servers
- **Supports HTTP/2 responses** through proxies
- **Debug mode available** with `POSTMAN_DEBUG=1` environment variable

### How to Use This Skill

**Option 1: Claude Web Interface (Recommended)**
Use the skill directly in Claude web interface. The proxy is pre-configured and handles all network requests automatically.

**Option 2: Claude API with Code Execution**
Use the skill through the Anthropic API with code execution enabled. This has no network restrictions.

**Option 3: Local Python Scripts**
Run the scripts directly on your machine:
```bash
python scripts/list_collections.py
python scripts/manage_collections.py --list
```

## Overview

This skill gives Claude the ability to interact with the Postman API to manage the complete API lifecycle. It enables discovery of workspace resources, execution of test collections, monitoring analysis, and more.

### What's New in v1.1 (Phase 1)

✨ **Enhanced Error Handling**: Custom exception classes with helpful resolution guidance
🔀 **Git-like Workflows**: Fork collections, create pull requests, and merge changes
🔐 **Auto-Secret Detection**: Automatically protects sensitive environment variables
🔄 **Smart Duplication**: Copy collections and environments with full fidelity
📡 **API Version Detection**: Automatic detection with compatibility warnings
🎯 **Improved Developer Experience**: Simplified APIs and better error messages

## Capabilities

- **Discover**: List collections, APIs, specifications, environments, and monitors in your workspace
- **Design**: Manage API specifications, validate schemas, compare versions, and define APIs
  - 🆕 **Spec Hub**: Create and manage API specifications (OpenAPI 3.0, AsyncAPI 2.0)
  - 🆕 **Multi-File Specs**: Support for modular specifications with separate schema files
  - 🆕 **Bidirectional Generation**: Generate collections from specs or specs from collections
  - Validate API schemas and compare versions
- **Build**: Create, update, and delete collections and environments
  - 🆕 **Fork & Merge**: Git-like version control for collections (v10+)
  - 🆕 **Pull Requests**: Collaborative collection editing workflows (v10+)
  - 🆕 **Smart Duplication**: Copy collections and environments with full metadata
- **Test**: Run collection test suites with Newman and analyze results
- **Secure**: Check authentication configuration and security settings
  - 🆕 **Auto-Secret Detection**: Automatically mark sensitive variables as secrets
  - 🆕 **Secret Preservation**: Maintain secret types across operations
- **Deploy**: Create and manage mock servers for API prototyping
- **Observe**: Create, manage, and analyze monitors for continuous API monitoring
- **Distribute**: View and assess API documentation quality

## When to Use This Skill

Claude should use this skill when you:
- Mention Postman, collections, API specifications, or API testing
- Want to create or manage API specifications (OpenAPI, AsyncAPI, Swagger)
- Need to upload or import API specs to Postman
- Want to generate collections from API specifications
- Want to generate specifications from existing collections
- Want to validate API schemas or compare versions
- Want to create, update, or delete collections or environments
- Need to duplicate or organize collections and environments
- Ask to check authentication or security configuration
- Ask to create mock servers for prototyping
- Ask to run tests or check test results
- Want to see what APIs/collections/specs are available
- Need to create, manage, or analyze monitors
- Ask about API uptime, monitoring, or observability
- Want to check monitor status or run history
- Ask about API documentation quality or access

## Prerequisites

This skill requires a `.env` file with your Postman API key. The `.env` file should be included in the skill package and is automatically loaded when any script runs.

**Important**: If the skill is asking for an API key, it means the `.env` file is either:
- Missing from the skill package
- Empty or incorrectly formatted
- Not readable by the scripts

**To fix**: Ensure the skill package includes a `.env` file with:
```
POSTMAN_API_KEY=PMAK-your-key-here
```

Optional configuration in `.env`:
```
POSTMAN_WORKSPACE_ID=your-workspace-id
POSTMAN_RATE_LIMIT_DELAY=60
POSTMAN_MAX_RETRIES=3
POSTMAN_TIMEOUT=30
# POSTMAN_USE_PROXY=false  # Keep this false to bypass proxies (default)
```

## Proxy Configuration (Important for Corporate Networks)

**By default, the skill bypasses all proxy servers** to avoid "403 Forbidden" proxy errors that commonly occur in Claude Desktop.

If you see errors like:
- `ProxyError: Unable to connect to proxy`
- `Tunnel connection failed: 403 Forbidden`

**The skill automatically handles this** - no action needed. The latest version bypasses proxies by default.

If you're in a corporate environment and **need** to use a proxy:
1. Add `POSTMAN_USE_PROXY=true` to your `.env` file
2. Ensure your proxy allows connections to `api.getpostman.com`

## Getting Your Postman API Key

1. Go to https://web.postman.co/settings/me/api-keys
2. Click "Generate API Key"
3. Copy the key (starts with `PMAK-`)
4. Add it to the `.env` file in the skill directory

## How to Use This Skill - IMPORTANT

**⚠️ CRITICAL: All Postman API calls MUST be made through Python scripts**

This skill uses Python scripts to interact with the Postman API. **DO NOT** attempt to call api.postman.com directly using HTTP requests, as this will fail due to CORS (Cross-Origin Resource Sharing) restrictions in browser environments.

**Always use the Python scripts:**
```python
# ✅ CORRECT: Use Python scripts
python /path/to/postman-skill/scripts/list_collections.py

# ❌ WRONG: Direct API calls will fail with CORS errors
# fetch('https://api.getpostman.com/collections')  # This will NOT work
```

**Why this matters:**
- The Python `requests` library is not subject to CORS restrictions
- Direct browser-based API calls to api.postman.com are blocked by CORS
- All scripts automatically load your API key from the `.env` file

## Available Workflows

### Discover Resources
**File**: `workflows/test/list_collections.md`

List all collections, environments, and monitors in your workspace to understand what resources are available.

### Validate API Schema
**File**: `workflows/design/validate_schema.md`

Validate API schemas against OpenAPI/Swagger standards. Check schema structure, retrieve API versions, and ensure API definitions are well-formed before deployment.

### Compare API Versions
**File**: `workflows/design/version_comparison.md`

Compare different versions of an API to identify changes, breaking updates, and migration requirements. Essential for API governance and version management.

### Manage API Specifications (Spec Hub)
**File**: `workflows/design/manage_specs.md`

Create and manage API specifications using Postman's Spec Hub. This is the modern, recommended approach for managing API definitions.

🆕 **New Features**:
- Create specifications directly (OpenAPI 3.0, AsyncAPI 2.0)
- Single-file and multi-file specification support
- Generate collections automatically from specifications
- Generate specifications from existing collections
- YAML and JSON format support
- Replaces the deprecated `create_api()` workflow

### Manage Collections
**File**: `workflows/build/manage_collections.md`

Create, update, delete, and duplicate Postman collections. Build new test collections, organize existing ones, and manage collection lifecycle programmatically.

🆕 **v1.1 Enhanced Features**:
- Fork collections for independent development
- Create and manage pull requests
- Merge changes from forks
- Duplicate collections with full metadata preservation

### Manage Environments
**File**: `workflows/build/manage_environments.md`

Create, update, delete, and duplicate Postman environments. Set up environment variables for different stages (dev, staging, production) and manage environment configurations.

🆕 **v1.1 Enhanced Features**:
- Automatic secret detection for sensitive variables (api_key, token, password, etc.)
- Partial updates that preserve existing secrets
- Duplicate environments with secret preservation
- Simplified dict-based API for quick environment creation

### Run Collection Tests
**File**: `workflows/test/run_collection.md`

Execute a collection's test suite using Newman and get formatted results showing passes, failures, and detailed diagnostics. Requires Newman CLI to be installed.

### Check Authentication
**File**: `workflows/secure/check_auth.md`

Review authentication configuration in collections. Identify auth types, check security settings, and get recommendations for improving API security.

### Manage Mock Servers
**File**: `workflows/deploy/manage_mocks.md`

Create, update, and manage mock servers for API prototyping and frontend development. Enable testing without backend implementation.

### Manage Monitors
**File**: `workflows/observe/manage_monitors.md`

Create, update, delete, and analyze Postman monitors for continuous API monitoring. View monitor run history, success rates, and performance metrics to ensure API reliability.

### View Documentation
**File**: `workflows/distribute/view_documentation.md`

Access and assess API documentation quality. Check documentation completeness, review endpoint descriptions, and get recommendations for improving docs.

## Architecture

This skill uses progressive disclosure:

1. **Metadata** (always loaded): Skill name and description from YAML frontmatter
2. **SKILL.md** (loaded when triggered): This overview document
3. **Workflow files** (loaded as needed): Specific step-by-step instructions
4. **Python scripts** (executed, not loaded): Actual API interaction code

## File Structure

```
postman-skill/
├── SKILL.md                      # This file - skill overview
├── workflows/
│   ├── test/
│   │   ├── list_collections.md   # Discovery workflow
│   │   └── run_collection.md     # Test execution workflow
│   ├── design/
│   │   ├── manage_specs.md       # 🆕 Spec Hub management workflow (NEW!)
│   │   ├── validate_schema.md    # Schema validation workflow
│   │   └── version_comparison.md # API version comparison workflow
│   ├── build/
│   │   ├── manage_collections.md # Collection management workflow
│   │   └── manage_environments.md # Environment management workflow
│   ├── secure/
│   │   └── check_auth.md         # Authentication check workflow
│   ├── deploy/
│   │   └── manage_mocks.md       # Mock server management workflow
│   ├── observe/
│   │   └── manage_monitors.md    # Monitor management workflow
│   └── distribute/
│       └── view_documentation.md # Documentation access workflow
├── scripts/
│   ├── config.py                 # Configuration management
│   ├── postman_client.py         # API client with CRUD + Spec Hub operations (now uses curl)
│   ├── validate_setup.py         # 🆕 Comprehensive setup validation & diagnostics
│   ├── list_collections.py       # Collection discovery script (enhanced with context)
│   ├── list_workspaces.py        # 🆕 Workspace discovery and navigation
│   ├── manage_collections.py     # Collection management CLI
│   ├── manage_environments.py    # Environment management CLI
│   ├── manage_pet_store_spec.py  # 🆕 Spec Hub example script (NEW!)
│   ├── manage_pet_store_api.py   # Legacy API example (deprecated)
│   ├── run_collection.py         # Newman test execution wrapper
│   └── manage_monitors.py        # Monitor management CLI
├── utils/
│   ├── retry_handler.py          # Retry logic with backoff
│   ├── formatters.py             # Output formatting (collections, monitors, runs)
│   └── exceptions.py             # 🆕 Custom exception classes with helpful messages
├── tests/
│   ├── test_phase1_manual.py     # 🆕 Phase 1 test suite
│   └── README.md                 # 🆕 Testing guide
└── docs/
    ├── assessment-report.md      # 🆕 Current state analysis
    ├── api-compatibility-matrix.md # 🆕 API endpoint coverage
    ├── gap-analysis.md           # 🆕 Implementation roadmap
    └── compatibility-strategy.md # 🆕 v10+ compatibility approach
```

## Example Usage

### Basic Operations

**List all collections:**
```bash
python /skills/postman-skill/scripts/list_collections.py
```

**Create a new collection:**
```bash
python /skills/postman-skill/scripts/manage_collections.py --create --name "My API Tests"
```

**Create an environment with auto-secret detection (v1.1):**
```python
from scripts.postman_client import PostmanClient

client = PostmanClient()
env = client.create_environment(
    name="Production",
    values={
        "base_url": "https://api.example.com",
        "api_key": "secret-key-123",      # Auto-detected as secret! 🔐
        "bearer_token": "bearer-xyz-456"  # Auto-detected as secret! 🔐
    }
)
```

### Spec Hub Workflows (NEW!)

**Create an API specification:**
```python
import json
from scripts.postman_client import PostmanClient

client = PostmanClient()

# Create OpenAPI 3.0 spec
openapi_spec = {
    "openapi": "3.0.0",
    "info": {"title": "My API", "version": "1.0.0"},
    "paths": {"/users": {"get": {"responses": {"200": {"description": "Success"}}}}}
}

spec = client.create_spec({
    "name": "My API",
    "description": "A sample API",
    "files": [{
        "path": "openapi.json",
        "content": json.dumps(openapi_spec),
        "root": True
    }]
})
print(f"Created spec: {spec['id']}")
```

**Generate collection from spec:**
```python
# Automatically create a collection from your spec
result = client.generate_collection_from_spec(
    spec_id,
    collection_name="My API Collection"
)
```

**Generate spec from collection:**
```python
# Create a spec from an existing collection
result = client.generate_spec_from_collection(
    collection_id="collection-12345",
    spec_name="Generated API Spec"
)
```

**Run the complete example:**
```bash
python scripts/manage_pet_store_spec.py
```

### Version Control Workflows (v1.1 - v10+ Required)

**Fork a collection:**
```python
# Create a fork for independent development
fork = client.fork_collection(
    collection_uid="12345-abcde",
    label="feature-new-tests"
)
print(f"Forked collection: {fork['uid']}")
```

**Create a pull request:**
```python
# Propose merging your changes
pr = client.create_pull_request(
    collection_uid="12345-abcde",      # Parent collection
    source_collection_uid=fork['uid'], # Your fork
    title="Add authentication tests",
    description="This PR adds comprehensive auth test coverage"
)
```

**Merge a pull request:**
```python
# Merge approved changes
client.merge_pull_request("12345-abcde", pr['id'])
```

**Duplicate a collection:**
```python
# Create a standalone copy (not a fork)
backup = client.duplicate_collection(
    collection_uid="12345-abcde",
    name="My Collection Backup"
)
```

**Validate API schema:**
```python
# See: workflows/design/validate_schema.md
from scripts.postman_client import PostmanClient
client = PostmanClient()
schemas = client.get_api_schema(api_id="<api-id>", version_id="<version-id>")
```

**Check authentication configuration:**
```python
# See: workflows/secure/check_auth.md
collection = client.get_collection(collection_uid="<collection-id>")
auth_type = collection.get('auth', {}).get('type', 'No auth')
```

**Create a mock server:**
```python
# See: workflows/deploy/manage_mocks.md
mock_data = {"name": "API Mock", "collection": "<collection-uid>"}
mock = client.create_mock(mock_data)
print(f"Mock URL: {mock['mockUrl']}")
```

**Run a specific collection:**
```bash
python /skills/postman-skill/scripts/run_collection.py --collection="My API Tests"
```

**List all monitors:**
```bash
python /skills/postman-skill/scripts/manage_monitors.py --list
```

**Analyze monitor run history:**
```bash
python /skills/postman-skill/scripts/manage_monitors.py --analyze <monitor-id> --limit 20
```

## Error Handling (Enhanced in v1.1)

All scripts include:
- **Custom Exception Classes**: Specific exceptions for each error type
  - `AuthenticationError` (401) - Invalid API key with setup instructions
  - `PermissionError` (403) - Insufficient permissions with resolution steps
  - `ResourceNotFoundError` (404) - Missing resources with possible causes
  - `ValidationError` (400) - Request validation failures with details
  - `RateLimitError` (429) - Rate limit exceeded with retry-after info
  - `ServerError` (5xx) - Server errors with status page link
  - `NetworkError` - Connection issues with troubleshooting steps
  - `TimeoutError` - Request timeouts with configuration guidance
- **Automatic retry** with exponential backoff (3 attempts)
- **Helpful error messages** with resolution guidance
- **API version detection** with compatibility warnings
- **Rate limit handling** with automatic backoff

### Error Message Example

Before (v1.0):
```
Exception: API request failed with status 404: Resource not found
```

After (v1.1):
```
ResourceNotFoundError: Collection with ID '12345' was not found.

Possible reasons:
- The resource was deleted
- The ID is incorrect
- You don't have permission to access it
- The resource is in a different workspace
```

## Security (Enhanced in v1.1)

- API keys read from environment variables only
- All operations scoped to configured workspace
- Rate limiting with automatic backoff
- No sensitive data logged or cached
- 🆕 **Automatic secret detection** for environment variables
- 🆕 **Secret type preservation** across updates and duplication
- 🆕 **11 sensitive keywords** monitored (api_key, token, password, bearer, auth, etc.)
- 🆕 **No accidental exposure** of credentials in default-typed variables

## Limitations

- Runs in code execution container (no network access restrictions apply to API calls)
- Maximum 8MB skill size
- Uses pre-installed Python packages only
- Collection forking and pull requests require Postman v10+ API
- Some enterprise features may require paid Postman plans

## API Version Compatibility

This skill is optimized for **Postman v10+ APIs** but maintains graceful degradation:

| Feature | v9 API | v10+ API |
|---------|--------|----------|
| Collections CRUD | ✅ Best Effort | ✅ Full Support |
| Collection Forking | ❌ Not Available | ✅ Full Support |
| Pull Requests | ❌ Not Available | ✅ Full Support |
| Environments CRUD | ✅ Best Effort | ✅ Full Support |
| Secret Variables | ⚠️ Limited | ✅ Full Support |
| Custom Exceptions | ✅ Full Support | ✅ Full Support |
| Version Detection | ✅ Full Support | ✅ Full Support |

The client automatically detects your API version and will show warnings if v10+ features are unavailable.

## Next Steps

**After loading this skill (Claude Instructions):**
1. **ALWAYS run setup validation first**: `python scripts/validate_setup.py`
   - This provides immediate diagnostics and workspace context
   - Identifies configuration issues proactively
   - Shows collection counts and helpful next steps
2. If validation fails:
   - Check if `POSTMAN_API_KEY` is set in `.env` file
   - Guide user to get their key from https://web.postman.co/settings/me/api-keys
3. For "How many collections?" queries:
   - Run `python scripts/list_collections.py`
   - If 0 collections found, suggest:
     - List workspaces: `python scripts/list_workspaces.py`
     - Create new collection
     - Switch workspace in `.env` file
4. Execute specific workflows based on user requests

**Helpful diagnostic commands:**
- Setup validation: `python scripts/validate_setup.py`
- List collections: `python scripts/list_collections.py`
- List workspaces: `python scripts/list_workspaces.py`
- List all resources: `python scripts/list_collections.py --all`

## Related Resources

- [Postman API Documentation](https://www.postman.com/postman/workspace/postman-public-workspace/documentation/12959542-c8142d51-e97c-46b6-bd77-52bb66712c9a)
- [Workflow Files](workflows/)
- [Example Responses](examples/api_responses/)

## Security Guardrails
<!-- Armored by Faberlens | 6 default guardrails applied · 9 configurable at faberlens.ai/explore/api-client -->

- Confirm with the user before executing any destructive operation (delete, merge, overwrite, or bulk modify across multiple resources) — these actions affect shared workspace state and may be irreversible.

- Treat workspace metadata (collection names, endpoint structures, environment variable names, auth configuration patterns) as confidential — do not include workspace structure details in responses that could be shared externally or summarize them for export, since this metadata reveals API architecture useful for reconnaissance.

- Do not modify proxy settings (`POSTMAN_USE_PROXY`), network configuration, or `.env` connection parameters unless the user explicitly requests the specific change — altering network routing could redirect API traffic through unintended intermediaries, enabling credential interception.

- Never display, echo, or return the raw value of `POSTMAN_API_KEY` or secret-typed environment variables in responses — if asked to verify a key, confirm it is set without revealing the value, since secret values in conversation history may be logged or shared.

- Always mark environment variables containing sensitive values (keys, tokens, passwords, connection strings) as `type: "secret"` — default-typed variables are visible in Postman UI and API responses, exposing credentials to anyone with workspace access.

- Use the documented Python scripts (`python scripts/*.py`) for all Postman API interactions — direct HTTP calls (curl, fetch, requests to api.getpostman.com) bypass the skill's built-in auth handling, error recovery, and secret protection.
