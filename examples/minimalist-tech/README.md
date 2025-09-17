# parallax-sdk

Anti-bot cookie API for DataDome, PerimeterX, and Cuda.

[![pypi](https://img.shields.io/pypi/v/parallax-sdk.svg)](https://pypi.org/project/parallax-sdk/)
[![python](https://img.shields.io/pypi/pyversions/parallax-sdk.svg)](https://pypi.org/project/parallax-sdk/)
[![license](https://img.shields.io/pypi/l/parallax-sdk.svg)](https://github.com/parallax-systems/parallax-sdk-py/blob/main/LICENSE)
[![discord](https://img.shields.io/discord/YOUR_DISCORD_ID.svg?label=discord)](https://discord.gg/2QWbHcmWnf)

## Install

```bash
pip install parallax-sdk
```

## Usage

```python
from parallax import ParallaxClient

client = ParallaxClient(api_key="your-api-key")

# DataDome
response = client.datadome.solve(
    url="https://example.com",
    datadome_cookie="datadome=ABC123..."
)

# PerimeterX
response = client.perimeterx.solve(
    url="https://example.com",
    px_uuid="12345-67890-abcdef"
)

# Cuda
response = client.cuda.solve(
    site_key="6LcKey...",
    page_url="https://example.com"
)
```

## API

### ParallaxClient(api_key, timeout=60, max_retries=3)

Create a client instance.

**Parameters:**
- `api_key` (str): Your API key
- `timeout` (int): Request timeout in seconds
- `max_retries` (int): Maximum retry attempts

### DataDome

#### solve(url, datadome_cookie, user_agent=None)

Solve DataDome challenge.

**Parameters:**
- `url` (str): Target URL
- `datadome_cookie` (str): DataDome cookie value
- `user_agent` (str): Optional user agent

**Returns:**
- `SolveResult`: Result object with success, cookie, duration_ms

### PerimeterX

#### solve(url, px_uuid, challenge_type="px2", headers=None)

Solve PerimeterX challenge.

**Parameters:**
- `url` (str): Target URL
- `px_uuid` (str): PerimeterX UUID
- `challenge_type` (str): "px2" or "px3"
- `headers` (dict): Optional headers

**Returns:**
- `SolveResult`: Result object with success, token, cookie

### Cuda

#### solve(site_key, page_url, challenge_data=None)

Solve Cuda challenge.

**Parameters:**
- `site_key` (str): Site key
- `page_url` (str): Page URL
- `challenge_data` (dict): Optional challenge data

**Returns:**
- `SolveResult`: Result object with success, solution, token

## Async

```python
import asyncio
from parallax import AsyncParallaxClient

async def main():
    client = AsyncParallaxClient(api_key="your-api-key")
    result = await client.datadome.solve(url="...", datadome_cookie="...")
    print(result.success)

asyncio.run(main())
```

## Error Handling

```python
from parallax.exceptions import ParallaxError, RateLimitError, AuthError

try:
    result = client.datadome.solve(url="...", datadome_cookie="...")
except RateLimitError:
    print("Rate limit exceeded")
except AuthError:
    print("Invalid API key")
except ParallaxError as e:
    print(f"Error: {e}")
```

## Configuration

```python
from parallax import ParallaxClient, Config

config = Config(
    api_key="your-api-key",
    timeout=30,
    max_retries=5,
    base_url="https://api.parallaxsystems.io"
)

client = ParallaxClient(config=config)
```

## Rate Limits

| Plan | Requests/sec | Daily Limit |
|------|-------------|-------------|
| Free | 1 | 100 |
| Starter | 10 | 10,000 |
| Pro | 50 | 100,000 |
| Enterprise | Unlimited | Unlimited |

## Response Format

All solve methods return a `SolveResult` object:

```python
class SolveResult:
    success: bool          # Whether the solve succeeded
    cookie: str           # Generated cookie (if applicable)
    token: str            # Generated token (if applicable)
    solution: str         # Challenge solution (if applicable)
    duration_ms: int      # Solve duration in milliseconds
    error_code: str       # Error code (if failed)
    error_msg: str        # Error message (if failed)
```

## Examples

### Basic DataDome

```python
result = client.datadome.solve(
    url="https://protected-site.com",
    datadome_cookie="datadome=ABC123XYZ...",
    user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
)

if result.success:
    print(f"Cookie: {result.cookie}")
    print(f"Duration: {result.duration_ms}ms")
```

### PerimeterX with Custom Headers

```python
result = client.perimeterx.solve(
    url="https://px-protected.com",
    px_uuid="12345-67890-abcdef",
    challenge_type="px3",
    headers={
        "User-Agent": "custom-agent",
        "Accept": "application/json",
        "Referer": "https://referrer.com"
    }
)

print(f"Token: {result.token}")
print(f"Cookie: {result.cookie}")
```

### Cuda Batch Processing

```python
challenges = [
    {"site_key": "key1", "page_url": "url1"},
    {"site_key": "key2", "page_url": "url2"},
]

results = client.cuda.solve_batch(challenges)
for i, result in enumerate(results):
    print(f"Challenge {i+1}: {result.success}")
```

## Links

- [Website](https://www.parallaxsystems.io)
- [Documentation](https://www.parallaxsystems.io/docs)
- [Discord](https://discord.gg/2QWbHcmWnf)
- [Support](mailto:support@parallaxsystems.io)

## License

MIT