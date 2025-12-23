# Frigate LLM Notification v0.7 [iOS-aware] - Intelligent Security Camera Automation

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2024.1%2B-blue.svg)](https://www.home-assistant.io/)
[![Frigate](https://img.shields.io/badge/Frigate-0.13%2B-green.svg)](https://github.com/blakeblackshear/frigate)
[![LLM Vision](https://img.shields.io/badge/LLM%20Vision-Integration-orange.svg)](https://github.com/valentinfrlch/ha-llmvision)

An advanced Home Assistant automation blueprint that combines **Frigate NVR**, **LLM Vision AI**, and **interactive mobile notifications** to create intelligent, context-aware security alerts with actionable buttons. Now with native **iOS support**.

## 🌟 Key Features

### 🍎 iOS & Android Support
- **Native iOS Streaming**: Uses HLS (`.m3u8`) for iOS devices and MP4 for Android.
- **Device Specific Attachments**: Automatically serves the correct media format based on the target device.
- **Dedicated iOS Device Selector**: Easily categorize your notify devices.

### 🎯 Interactive Notifications
- **📱 3 Customizable Buttons** per notification type (New Event + End Event)
- **🔇 Smart Silence Feature** with custom timer input (5-120 minutes)
- **📞 Direct Call Integration** with configurable phone numbers
- **📸 Quick Actions**: View Live, View Snapshot, View Clip (iOS/Android aware)
- **🏠 Custom Actions**: Execute any Home Assistant automation

### 🤖 Advanced AI Analysis
- **Dual Analysis**: Initial snapshot + comprehensive video clip analysis
- **Multiple LLM Support**: Gemini 2.0/2.5, Claude 3.5/3.7, GPT-4o, Llama 4
- **Person-Verified Detection**: Handles Frigate's enhanced person detection
- **Memory Integration**: Uses LLM Vision timeline and memory features
- **Custom Prompts**: Tailored AI analysis for your specific needs

### 🛡️ Smart Filtering System
- **Zone Vacancy Detection**: Only alert when specific people are away
- **Person Presence Validation**: Check if family members are home/away
- **Custom Conditions**: Add complex logic conditions
- **Sub-label Support**: Advanced filtering with Frigate sub-labels

### 🔧 Features
- **Optional Downloader**: Choose between direct URLs or local file storage
- **Parallel Processing**: Handle multiple camera events simultaneously
- **Enhanced Error Handling**: Robust retry logic and safe template processing
- **Cooldown Management**: Prevent notification spam with configurable delays

## 🛠️ Prerequisites

### Required Components

1. **[Frigate Integration](https://github.com/blakeblackshear/frigate-hass-integration)**
   ```yaml
   # Frigate 0.13+ with MQTT setup
   # Camera zones configured
   # Object detection enabled
   ```

2. **[LLM Vision Integration](https://github.com/valentinfrlch/ha-llmvision)**
   ```yaml
   # Provider configured (OpenAI, Anthropic, Google, etc.)
   # API keys properly set
   # Memory/Timeline optional but recommended
   ```

3. **[Home Assistant Companion App](https://companion.home-assistant.io/)**
   ```yaml
   # Android/iOS app installed
   # Notification permissions enabled
   # Actionable notifications supported
   ```

4. **[Downloader Integration](https://www.home-assistant.io/integrations/downloader/)** (Optional)
   ```yaml
   # Built-in HA integration
   # Directory permissions configured
   # Storage space available
   ```

## 📦 Installation & Migration

### 🆕 **New Installation (v0.7)**

#### 1. Import Blueprint
```bash
# Method 1: Direct URL Import
https://raw.githubusercontent.com/willhaggan/HA_Frigate_VLLM_Notification/main/Latest.yaml

# Method 2: HA UI
Settings → Automations & Scenes → Blueprints → Import Blueprint
```

#### 2. Basic Configuration
```yaml
# Minimal setup for testing
Camera: camera.front_door
Objects: ["person"]
LLM Provider: your_llm_vision_provider
Notification Devices: [your_mobile_device]
iOS Devices: [your_iphone] # Select iOS devices here for HLS support
Button 1: "View Live" (VIEW_LIVE)
Button 2: "Silence" (SILENCE)
```

#### 3. Test Your Setup
1. **Create automation** with minimal settings above
2. **Trigger person detection** at your camera
3. **Verify notification** appears with working buttons
4. **Test silence functionality** with custom timer

---

## 🎛️ Configuration Examples

### Example 1: Basic Front Door Monitoring (Mixed Devices)
```yaml
# Simple person detection with interactive buttons
Frigate Camera: camera.front_door
Required Objects: ["person"]
Zones: ["front_porch"]
Severity: ["alert", "detection"]

# Device Configuration
Notify Devices: ["mobile_app_iphone", "mobile_app_android"]
iOS Devices: ["mobile_app_iphone"] # Important for correct video format

# Interactive Buttons (New Event)
Button 1: "View Live" → VIEW_LIVE
Button 2: "Silence" → SILENCE  
Button 3: "Call Security" → CALL

# AI Analysis
Snapshot Prompt: "Describe who is at the front door and what they appear to be doing"
Clip Prompt: "Analyze the person's behavior. Are they delivering something, visiting, or suspicious?"
```

## 📱 Interactive Button Features

### Available Button Actions

| Action | Description | Use Case |
|--------|-------------|----------|
| `VIEW_LIVE` | Open live camera feed | Quick security check |
| `VIEW_SNAPSHOT` | View event snapshot | See what triggered alert |
| `VIEW_CLIP` | View full video clip | Review complete event (iOS/Android aware) |
| `SILENCE` | Pause notifications | Temporary quiet period |
| `CALL` | Dial phone number | Contact security/family |

### Smart Silence Feature
```yaml
# User interaction examples:
Tap "Silence" → 5 minutes (default)
Type "30" + Send → 30 minutes
Type "120" + Send → 2 hours
```

## 🎯 Advanced Template Variables

Use these in custom messages and AI prompts:

### Event Information
```jinja2
{{input_objects}}      # Objects required: ["person", "car"]
{{objects}}            # Objects detected: ["person"]
{{camera_name}}        # Camera name: "Front Door"
{{zone_names}}         # Zones required: ["front_porch"]
{{video}}              # MP4 Clip URL (Android)
{{video_ios}}          # HLS Clip URL (iOS)
{{detections[0]}}      # Event ID: "1642095978.456789-abc123"
```

## 🚀 Performance Tips

### AI Model Selection
```yaml
# Fast models (real-time alerts):
- gpt-4o-mini
- gemini-2.0-flash-lite  
- claude-3-5-haiku-latest

# Quality models (detailed analysis):
- gpt-4o
- claude-3-7-sonnet-latest
- gemini-2.0-flash
- gemini-2.5-pro
- Llama-4-Maverick
```

### Token Optimization
```yaml
# Snapshot Analysis: 50-100 tokens (quick description)
# Clip Analysis: 100-200 tokens (detailed analysis)
# Balance: Speed vs Detail based on your needs
```

## 🤝 Contributing

We welcome contributions! Here's how to help:

### Bug Reports
- Use GitHub Issues with detailed error logs
- Include Home Assistant and integration versions
- Provide configuration examples that reproduce the issue

---

**Version**: v0.7  
**Author**: whag  
**Last Updated**: December 2025  
**Compatibility**: Home Assistant 2024.1+, Frigate 0.13+

## 📚 Quick Links

- [Blueprint File](Latest.yaml) - Ready to import
- [GitHub Issues](../../issues) - Bug reports and feature requests  
- [Home Assistant Community](https://community.home-assistant.io/) - General discussion
- [Frigate Documentation](https://docs.frigate.video/) - Camera setup guide
- [LLM Vision Setup](https://github.com/valentinfrlch/ha-llmvision) - AI provider configuration
