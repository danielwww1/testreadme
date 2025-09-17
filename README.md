<div align="center">

# Parallax Systems SDK

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Discord](https://img.shields.io/badge/Discord-Join%20Us-7289da?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/2QWbHcmWnf)
[![PyPI Version](https://img.shields.io/pypi/v/parallax-sdk?style=flat-square&color=orange)](https://pypi.org/project/parallax-sdk/)
[![Downloads](https://img.shields.io/pypi/dm/parallax-sdk?style=flat-square&color=red)](https://pypi.org/project/parallax-sdk/)
[![Documentation](https://img.shields.io/badge/Docs-Available-brightgreen?style=flat-square)](https://www.parallaxsystems.io)

**Enterprise-grade Anti-Bot Cookie API for DataDome, PerimeterX, and Cuda**

[Website](https://www.parallaxsystems.io) • [Documentation](https://www.parallaxsystems.io/docs) • [Discord](https://discord.gg/2QWbHcmWnf) • [Examples](#examples)

</div>

---

## 🚀 Overview

Parallax Systems SDK provides a powerful, streamlined solution for handling anti-bot challenges programmatically. No browser automation required - generate valid cookies and sensors through our high-performance API.

### Why Choose Parallax?

<table>
<tr>
<td width="33%" align="center">

**⚡ Lightning Fast**
Sub-second response times with optimized algorithms

</td>
<td width="33%" align="center">

**🔒 Enterprise Ready**
99.9% uptime SLA with dedicated support

</td>
<td width="33%" align="center">

**🎯 High Success Rate**
Industry-leading solve rates across all platforms

</td>
</tr>
</table>

## 📦 Installation

```bash
pip install parallax-sdk
```

For specific version:
```bash
pip install parallax-sdk==1.0.0
```

## ⚡ Quick Start

```python
from parallax import ParallaxClient

# Initialize client
client = ParallaxClient(api_key="your-api-key")

# Solve DataDome challenge
response = client.datadome.solve(
    url="https://example.com",
    datadome_cookie="datadome=ABC123..."
)

print(f"Cookie: {response.cookie}")
print(f"Success: {response.success}")
```

## ✨ Features

### Supported Platforms

<table>
<thead>
<tr>
<th>Platform</th>
<th>Status</th>
<th>Features</th>
<th>Success Rate</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>DataDome</strong></td>
<td>✅ Stable</td>
<td>Cookie Generation, Captcha Solving, Sensor Creation</td>
<td>99.8%</td>
</tr>
<tr>
<td><strong>PerimeterX</strong></td>
<td>✅ Stable</td>
<td>PX2, PX3 Challenges, Mobile Support</td>
<td>99.5%</td>
</tr>
<tr>
<td><strong>Cuda</strong></td>
<td>✅ Stable</td>
<td>Challenge Solving, Token Generation</td>
<td>99.7%</td>
</tr>
</tbody>
</table>

### Core Capabilities

- **🔄 Automatic Retries**: Smart retry logic with exponential backoff
- **📊 Analytics Dashboard**: Real-time monitoring and statistics
- **🌍 Global Infrastructure**: Low-latency endpoints worldwide
- **🛠️ Flexible Integration**: REST API, Python SDK, and webhooks
- **📱 Mobile Support**: iOS and Android challenge solving
- **🔐 Secure**: End-to-end encryption for all requests

## 📖 Usage Examples

### DataDome Integration

```python
from parallax import ParallaxClient
from parallax.exceptions import ParallaxError

client = ParallaxClient(api_key="your-api-key")

try:
    # Basic cookie generation
    result = client.datadome.solve(
        url="https://target-site.com",
        datadome_cookie="datadome=ABC123XYZ...",
        user_agent="Mozilla/5.0..."
    )

    # Use the cookie in your requests
    import requests

    session = requests.Session()
    session.cookies.set("datadome", result.cookie)
    response = session.get("https://target-site.com/api/data")

except ParallaxError as e:
    print(f"Error: {e.message}")
    print(f"Code: {e.code}")
```

### PerimeterX Handling

```python
# Handle PerimeterX challenges
result = client.perimeterx.solve(
    url="https://protected-site.com",
    px_uuid="12345-67890-abcdef",
    px_vid="vid_123456",
    challenge_type="px2",  # or "px3"
    headers={
        "User-Agent": "your-user-agent",
        "Accept": "application/json"
    }
)

# Extract tokens
print(f"PX Token: {result.token}")
print(f"PX Cookie: {result.cookie}")

# Advanced configuration
result = client.perimeterx.solve(
    url="https://protected-site.com",
    px_uuid="12345-67890-abcdef",
    config={
        "mobile": True,
        "app_id": "PX12345678",
        "timeout": 30,
        "retry_attempts": 3
    }
)
```

### Cuda Solutions

```python
# Solve Cuda challenges
result = client.cuda.solve(
    site_key="6LcKey...",
    page_url="https://cuda-protected.com",
    challenge_data={
        "type": "recaptcha_v2",
        "action": "submit"
    }
)

print(f"Solution: {result.solution}")
print(f"Token: {result.token}")

# Batch processing
challenges = [
    {"site_key": "key1", "url": "url1"},
    {"site_key": "key2", "url": "url2"},
]

results = client.cuda.solve_batch(challenges)
for idx, result in enumerate(results):
    print(f"Challenge {idx + 1}: {result.success}")
```

## 🔧 Advanced Configuration

### Client Options

```python
from parallax import ParallaxClient, Config

# Custom configuration
config = Config(
    api_key="your-api-key",
    timeout=60,  # Request timeout in seconds
    max_retries=5,  # Maximum retry attempts
    base_url="https://api.parallaxsystems.io",  # Custom endpoint
    debug=True  # Enable debug logging
)

client = ParallaxClient(config=config)
```

### Async Support

```python
import asyncio
from parallax import AsyncParallaxClient

async def main():
    client = AsyncParallaxClient(api_key="your-api-key")

    # Concurrent solving
    tasks = [
        client.datadome.solve(url="url1", cookie="cookie1"),
        client.datadome.solve(url="url2", cookie="cookie2"),
    ]

    results = await asyncio.gather(*tasks)
    for result in results:
        print(f"Success: {result.success}")

asyncio.run(main())
```

### Webhook Integration

```python
# Configure webhooks for async processing
client.webhooks.register(
    url="https://your-server.com/webhook",
    events=["challenge.solved", "challenge.failed"],
    secret="webhook-secret"
)

# Submit challenge for async processing
job = client.datadome.solve_async(
    url="https://example.com",
    datadome_cookie="cookie...",
    callback_url="https://your-server.com/callback"
)

print(f"Job ID: {job.id}")
print(f"Status: {job.status}")
```

## 📊 API Reference

### Core Methods

| Method | Description | Parameters | Returns |
|--------|-------------|------------|---------|
| `client.datadome.solve()` | Solve DataDome challenge | `url`, `datadome_cookie`, `user_agent` | `SolveResult` |
| `client.perimeterx.solve()` | Handle PerimeterX | `url`, `px_uuid`, `challenge_type` | `SolveResult` |
| `client.cuda.solve()` | Solve Cuda challenges | `site_key`, `page_url`, `challenge_data` | `SolveResult` |
| `client.stats.get()` | Get usage statistics | `date_from`, `date_to` | `StatsResponse` |

### Response Objects

```python
# SolveResult object
result.success      # bool: Whether solving succeeded
result.cookie       # str: Generated cookie value
result.token        # str: Challenge token
result.duration_ms  # int: Solving duration
result.metadata     # dict: Additional information

# Error handling
result.error_code   # str: Error identifier
result.error_msg    # str: Human-readable error
```

### Rate Limits

| Plan | Requests/Second | Daily Limit | Concurrent |
|------|----------------|-------------|------------|
| Free | 1 | 100 | 1 |
| Starter | 10 | 10,000 | 5 |
| Professional | 50 | 100,000 | 20 |
| Enterprise | Unlimited | Unlimited | Unlimited |

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

```bash
# Clone repository
git clone https://github.com/parallax-systems/parallax-sdk-py
cd parallax-sdk-py

# Install development dependencies
pip install -e ".[dev]"

# Run tests
pytest tests/
```

## 📚 Resources

- [Official Documentation](https://www.parallaxsystems.io/docs)
- [API Reference](https://www.parallaxsystems.io/api-reference)
- [Integration Examples](https://github.com/parallax-systems/examples)
- [Status Page](https://status.parallaxsystems.io)
- [Changelog](CHANGELOG.md)

## 💬 Support

Need help? We're here for you:

- 💬 [Discord Community](https://discord.gg/2QWbHcmWnf)
- 📧 [Email Support](mailto:support@parallaxsystems.io)
- 📖 [Knowledge Base](https://www.parallaxsystems.io/kb)
- 🐛 [Issue Tracker](https://github.com/parallax-systems/parallax-sdk-py/issues)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Made with ❤️ by [Parallax Systems](https://www.parallaxsystems.io)

[Website](https://www.parallaxsystems.io) • [Twitter](https://twitter.com/parallaxsystems) • [Discord](https://discord.gg/2QWbHcmWnf)

</div>