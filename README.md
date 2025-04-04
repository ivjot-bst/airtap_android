# AirTap Android Python SDK

![AirTap Android](https://airtap.ai/assets/images/airtap-logo.png)

## 📱 What is AirTap?

AirTap is a cloud-based platform that provides virtual Android environments controlled by AI agents. Instead of manually interacting with mobile apps, AirTap's AI agents intelligently operate Android applications on your behalf, executing complex workflows across multiple apps without requiring physical devices or manual intervention.

An "airtap" is a virtual, intelligent tap, swipe, or key-press triggered by an LLM agent in the cloud. The AI agent analyzes screens, understands context, and makes decisions about where and when to interact—just like a human would, but with the efficiency and precision of automation.

## ✨ Key Features

### 🧠 AI-Powered Interaction
- **Intelligent virtual touches**: AI decides where/when to tap, swipe, or type
- **Visual understanding**: LLMs analyze screen content to comprehend interface context
- **Autonomous decision-making**: Agents navigate complex app flows without predefined paths
- **Natural language instructions**: Define tasks in plain English instead of writing code

### ☁️ Cloud-Native Android
- **Fully virtualized**: Complete Android environments running in isolated cloud VMs
- **Off-device execution**: Actions execute inside secure sandboxes; your local devices stay untouched
- **Multiple device profiles**: Different Android versions, screen sizes, and configurations
- **Scalable infrastructure**: Run multiple instances simultaneously for parallel workflows

### 🔄 Cross-App Workflows
- **Seamless transitions**: The same agent can operate across multiple applications
- **Data sharing**: Transfer information between apps as part of workflows
- **Persistent context**: Maintain understanding throughout multi-step processes

## 🔍 AirTap Android Sandbox

The AirTap Android Sandbox provides a fully isolated Android Virtual Machine (VM), currently running Android 13 (API 33). This allows your code or LLM agents to stream, inspect, and control the Android environment for mobile tasks.

### Open-source projects to use Sandbox to control Android apps
- Python - https://github.com/ivjot-bst/airtap_android
- JavaScript - https://github.com/ivjot-bst/airtapAndroid

## 🚀 Getting Started

### 1. Get Your AirTap API Key

```bash
# Create an account at https://airtap.ai/
# Copy your key and set environment variable
export AIRTAP_API_KEY="<your-key>"
```

### 2. Clone the Repository

```bash
# Clone the AirTap Python repository
git clone https://github.com/ivjot-bst/airtap_android.git
cd airtap_android
pip install -r requirements.txt
```

### 3. Create Your First Virtual Device

```python
from airtap_android import AndroidSandbox

# Basic instance (defaults: 1280×720, 160 dpi)
phone = AndroidSandbox()

# Custom config
phone = AndroidSandbox(
    resolution=(1280, 720),
    dpi=320,
    webrtc=True
)
```

## Features & Usage Examples

### WebRTC Live Streaming

```python
# Stream the device screen
url = phone.stream.start()          # webrtc connection
print(f"View device at: {url}")

# Disable user input for view-only mode
url = phone.stream.start(view_only=True)

# Stop streaming
phone.stream.stop()
```

### Low-Level Device Control

When you need precise control over individual actions:

```python
# Manual control operations
phone = AndroidSandbox()

# Touch & gesture control
phone.tap(300, 900)
phone.double_tap(500, 500)
phone.swipe((100, 1600), (100, 400), duration_ms=600)
phone.long_press(540, 960, 1200)  # 1.2 seconds
phone.pinch_out((540, 960), 300)  # zoom

# Keyboard input
phone.type_text("Hello world")
phone.press_key("ENTER")
phone.press_combo(["CTRL", "A"])

# App management
phone.apps.install("com.spotify.music")
phone.apps.open("com.spotify.music/.MainActivity")
phone.apps.close("com.spotify.music")
package_list = phone.apps.list_installed()

# Visual Feedback
screenshot = phone.screenshot()
rec_id = phone.record.start()
phone.record.stop(rec_id)  

# Push a local file to the device
phone.files.push("local.txt", "/sdcard/Documents/local.txt")

# Pull a file from the device
phone.files.pull("/sdcard/Pictures/image.png", "downloaded_image.png")
```

### Other Interaction Methods

```python
# Shell Commands
# Get the list of packages
packages = phone.shell("pm list packages")

# Start an activity
phone.shell("am start -n com.android.settings/.Settings\$WifiSettingsActivity")

# Wait Utilities
# Wait for a different app
phone.wait_for_app("com.instagram.android", timeout_ms=15000)

# Sleep for a longer duration
phone.wait(3000)
```

## Use Cases

### E-commerce Assistant
- Analyzes products from various shopping apps
- Compares prices, features, and reviews with improved contextual awareness
- Facilitates purchase decisions based on user preferences
- Monitors price trends and advises on the best time to buy

### Social Media Management
- Evaluates content quality and potential engagement prior to publishing
- Tracks competitor actions with contextual insights
- Suggests content based on audience data
- Implements cross-platform posting strategies

### Travel Planning
- Searches across multiple travel sites while considering user preferences
- Compares intricate travel choices using various factors
- Handles end-to-end booking with inter-app functionality
- Observes price fluctuations and recommends optimal booking times

### Personal Assistant
- Schedules appointments with smart calendar management
- Manages finances by understanding spending habits
- Organizes tasks with contextual awareness of priorities
- Performs complex, adaptive workflows

## Security and Privacy

- **Isolated Environments**: Each virtual device runs in an isolated container
- **Best Practices**:
  - Store API keys in environment variables
  - Rotate API keys periodically
  - Use scoped permissions when possible
  - Close sandbox instances when done to free resources

## AirTap Technical Foundation

AirTap is built on top of:
- Android 13 (API level 33) running inside a KVM guest with hardware acceleration
- WebRTC for low-latency video streaming
- ADB for device control and file management
- LLM vision models for UI understanding and decision making
- Custom reasoning chains for domain-specific tasks

## ❓ Troubleshooting

### Common Issues

#### Connection Problems
- **Error**: Could not connect to Android device
  - Check that your API key is correctly set
  - Ensure you have a stable internet connection
  - Verify firewall settings are not blocking WebRTC connections

#### Performance Issues
- For slow responses, try reducing screen resolution:
  ```python
  phone = AndroidSandbox(resolution=(720, 1280), dpi=240)
  ```
- For memory-intensive apps, increase VM memory:
  ```python
  phone = AndroidSandbox(memory_mb=4096)
  ```

## 🔮 Next Steps

- Combine the Android sandbox capabilities to your domain to build fully autonomous mobile agents
- Browse example projects in the examples/ folder
- Join our community on Discord
- Sign up for production access at airtap.ai

## 📚 API Reference

For complete API documentation, see the API Reference.
- Python - https://github.com/ivjot-bst/airtap_android

## Project Structure

```
airtap_android_py/
├── airtap_android/         # Main package directory
├── docs/                   # Documentation
│   └── api/                # API reference
├── examples/               # Example scripts
│   ├── basic/              # Basic usage examples
│   ├── streaming/          # Streaming examples
│   ├── app_management/     # App management examples
│   └── advanced/           # Advanced usage examples
├── tests/                  # Unit tests
├── .github/                # GitHub workflows
├── .gitignore              # Git ignore rules
├── .pre-commit-config.yaml # Pre-commit hooks
├── CHANGELOG.md            # Version changelog
├── CONTRIBUTING.md         # Contribution guidelines
├── README.md               # This file
├── requirements.txt        # Dependencies
├── setup.py                # Package setup
└── pyproject.toml          # Project configuration
```


