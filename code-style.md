# Azure Functions MCP Lab Code Style Guide

## Naming Conventions

### General Naming Rules
- **Variables**: Use descriptive names written in `snake_case`. Shorter variables in contexts where scope and purpose are clear.
- **Functions**: Name functions using `snake_case` with a concise description of their action.
- **Classes**: Use `PascalCase` to name classes. Keep class names descriptive but concise.
- **Modules**: File names should be in `lowercase_with_underscores`.

### Examples
```python
# Variables
user_id = "12345"
connection_string = "Server=myServerAddress;Database=myDataBase;"

# Functions
def generate_documentation(agent_service, code_snippet):
    pass

# Classes
class SnippetManager:
    pass

# Module Name
src_folder/file_management.py
```

## Code Organization

### Structure
- Place Azure Function apps inside the `src/` directory.
- Use folders to organize files by functionality (e.g., infra, src, logging).
- Separate configuration files like `bicep` templates into an `infra/` directory.

### Example Directory Structure
```
project_root/
│
├── infra/
│   ├── main_template.bicep
│   ├── network_template.bicep
│
├── src/
│   ├── __init__.py
│   ├── manage_snippets.py
│   ├── logging_config.py
│
```

## Documentation Standards

### Style
- Use docstrings for modules, classes, and functions.
- Document the purpose and usage of functionalities concisely.

### Example
```python
def fetch_snippet(snippet_id):
    """
    Fetch a code snippet from the database based on the snippet ID.

    Parameters:
    - snippet_id (str): The ID of the code snippet to be retrieved.

    Returns:
    - dict: The code snippet data.
    """
```

## Error Handling

### Principles
- Implement try/except blocks to catch and handle exceptions.
- Use Python's built-in exceptions wherever applicable.
- Log exceptions using logging practices before re-raising or returning an error response.

### Example
```python
try:
    snippet = fetch_snippet(snippet_id)
except ValueError as e:
    logging.error(f"Invalid snippet ID: {snippet_id} - {str(e)}")
    raise
except Exception as e:
    logging.exception("An unexpected error occurred")
```

## Async/Await Patterns

### Use
- Employ async/await patterns for I/O-bound and high-level structured network code.

### Example
```python
import aiohttp

async def fetch_data_from_api(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.json()
```

## Logging Practices

### Configuration
- Configure logging to reduce verbosity for specific loggers like `azure`, `azure.core`, and `azure.ai.projects`.
- Use `INFO` level for general logs, `ERROR` or `WARNING` for error conditions, and `DEBUG` for detailed output when troubleshooting.

### Example
```python
import logging

logging.basicConfig(level=logging.INFO)
logging.getLogger('azure').setLevel(logging.WARNING)
logging.getLogger('azure.core').setLevel(logging.ERROR)
logging.getLogger('azure.ai.projects').setLevel(logging.CRITICAL)
```

## Type Hints

### Use
- Use Python type hints to clarify parameter types and return types of functions.

### Example
```python
def process_snippet(snippet_data: dict) -> str:
    """
    Process the given snippet data and return a formatted string.

    Parameters:
    - snippet_data (dict): The snippet information.

    Returns:
    - str: Processed snippet string.
    """
```

## Azure SDK Usage Patterns

### Best Practices
- Use the Azure SDK to interface with Azure resources efficiently.
- Utilize async methods provided by Azure SDK for network interactions.

### Example
```python
from azure.identity import DefaultAzureCredential
from azure.mgmt.resource import ResourceManagementClient

async def list_resources():
    credentials = DefaultAzureCredential()
    resource_client = ResourceManagementClient(credentials, "<subscription-id>")
    async for resource in resource_client.resources.list():
        print(resource.name)
```

This guide defines the essential coding standards and practices to be followed for the Azure Functions MCP Lab project. Implementing these standards will ensure readability, maintainability, and efficiency in your codebase.
