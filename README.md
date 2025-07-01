# Frigate LLM Notification - Intelligent Security Camera Automation

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2023.1%2B-blue.svg)](https://www.home-assistant.io/)
[![Frigate](https://img.shields.io/badge/Frigate-0.12%2B-green.svg)](https://github.com/blakeblackshear/frigate)
[![LLM Vision](https://img.shields.io/badge/LLM%20Vision-Integration-orange.svg)](https://github.com/valentinfrlch/ha-llmvision)

An advanced Home Assistant automation blueprint that combines **Frigate NVR**, **LLM Vision AI**, and **mobile notifications** to create intelligent, context-aware security alerts. This automation analyzes both snapshots and video clips using AI to provide detailed, actionable security notifications.

## 🌟 Features

### 🤖 AI-Powered Analysis
- **Dual Analysis**: Processes both initial snapshots and review clips
- **Multiple LLM Providers**: Supports OpenAI GPT, Claude, Gemini, and Llama models
- **Contextual Understanding**: AI describes what's happening, why, and what might happen next
- **Memory Integration**: Uses LLM Vision's memory and timeline features for enhanced context

### 📱 Smart Notifications
- **Rich Mobile Notifications**: Includes images, videos, and actionable buttons
- **Customizable Click Actions**: Direct links to camera feeds, downloaded files, or dashboards
- **Multiple Device Support**: Send notifications to multiple mobile devices
- **Notification Grouping**: Organizes alerts by camera for better management

### 🔧 Advanced Configuration
- **Zone-Based Filtering**: Only alert for specific areas of interest
- **Object Detection**: Filter alerts by specific objects (person, car, etc.)
- **Sub-label Support**: Advanced filtering with Frigate sub-labels
- **Cooldown Periods**: Prevents notification spam
- **Custom Actions**: Run additional automations on events

### 📁 Local File Management
- **Download Integration**: Stores Frigate files locally for improved reliability
- **Organized Storage**: Configurable directory structure for media files
- **Parallel Processing**: Handles multiple camera events simultaneously

## 🛠️ Prerequisites

Before using this automation, ensure you have the following components installed and configured:

### Required Integrations

1. **[Frigate Integration](https://github.com/blakeblackshear/frigate-hass-integration)**
   - Frigate NVR with MQTT broker setup
   - Camera configuration with zones and object detection
   - Integration installed and configured in Home Assistant

2. **[LLM Vision Integration](https://github.com/valentinfrlch/ha-llmvision)**
   - AI provider configured (OpenAI, Anthropic, Google, etc.)
   - Memory and timeline features (optional but recommended)
   - Integration installed and configured in Home Assistant

3. **[Downloader Integration](https://www.home-assistant.io/integrations/downloader/)**
   - Built-in Home Assistant integration
   - Directory permissions configured for file storage

4. **[Home Assistant Companion App](https://companion.home-assistant.io/)**
   - Installed on mobile devices for notifications
   - Notification permissions enabled

### System Requirements

- **Home Assistant**: Version 2023.1 or later
- **MQTT Broker**: Required for Frigate integration
- **Storage Space**: Adequate space for downloaded media files
- **Mobile Device**: Android or iOS with HA Companion App

## 📦 Installation

### Method 1: Blueprint Import (Recommended)

1. **Import the Blueprint**:
   ```
   https://github.com/willhaggan/HA_Frigate_VLLM_Notification/blob/main/Latest.yaml
   ```

2. **Navigate to Settings → Automations & Scenes → Blueprints**

3. **Click "Import Blueprint"** and paste the URL above

4. **Create a new automation** from the imported blueprint

### Method 2: Manual Installation

1. **Download** the `Latest.yaml` file from this repository

2. **Place the file** in your Home Assistant blueprints directory:
   ```
   config/blueprints/automation/frigate_llm_notification/
   ```

3. **Restart Home Assistant** or reload automations

4. **Create automation** from the blueprint in the UI

## ⚙️ Configuration

The blueprint provides extensive configuration options organized into logical groups:

### 🎥 Frigate Options
- **Camera Selection**: Choose which Frigate camera to monitor
- **Zone Configuration**: Select specific zones for monitoring
- **Object Filtering**: Filter alerts by detected objects
- **Sub-label Filtering**: Advanced filtering with custom labels

### 🤖 LLM Vision Provider
- **Provider Selection**: Choose your AI provider (OpenAI, Claude, Gemini, etc.)
- **Model Selection**: Select specific AI model for analysis
- **Performance Settings**: Configure tokens, temperature, and creativity

### 📸 Snapshot Analysis
- **AI Analysis Toggle**: Enable/disable AI analysis for snapshots
- **Custom Prompts**: Define how the AI should analyze images
- **Title Generation**: AI-generated notification titles
- **Memory Integration**: Use stored context for better analysis

### 🎬 Video Clip Analysis
- **Clip Processing**: Analyze video clips when events end
- **Frame Analysis**: Configure how many frames to analyze
- **Advanced Prompts**: Specialized prompts for video analysis
- **Performance Tuning**: Optimize for speed vs. quality

### 💾 Downloader Options
- **Directory Configuration**: Set up local storage paths
- **Download Timeouts**: Configure wait times for file downloads
- **Retry Logic**: Automatic retry on download failures

### 📱 Notification Settings
- **Device Selection**: Choose target mobile devices
- **Click Actions**: Configure notification tap behavior
- **Cooldown Periods**: Prevent notification spam
- **Grouping Options**: Organize notifications by camera

### 🔧 Custom Actions
- **New Event Actions**: Run additional automations on new events
- **End Event Actions**: Run actions when events complete
- **Integration Hooks**: Connect with other Home Assistant automations

## 📚 Documentation

- **[Quick Start Guide](docs/QUICK_START.md)** - Get up and running in 15 minutes
- **[Configuration Guide](docs/CONFIGURATION.md)** - Detailed configuration options
- **[Examples and Use Cases](docs/EXAMPLES.md)** - Real-world implementation examples
- **[Troubleshooting Guide](docs/TROUBLESHOOTING.md)** - Common issues and solutions

## 📝 Usage Examples

### Basic Security Monitoring
```yaml
# Configuration for basic person detection
Camera: camera.front_door
Zones: [front_porch, driveway]
Objects: [person]
AI Prompt: "Describe who is approaching the front door and what they appear to be doing."
```

### Advanced Vehicle Monitoring
```yaml
# Configuration for driveway vehicle detection
Camera: camera.driveway
Zones: [driveway, street]
Objects: [car, truck, motorcycle]
AI Prompt: "Analyze the vehicle activity. Is this a delivery, visitor, or potential security concern?"
```

### Multi-Zone Package Detection
```yaml
# Configuration for package delivery monitoring
Camera: camera.porch
Zones: [front_porch, steps]
Objects: [person, package]
AI Prompt: "Focus on package delivery activity. Describe if packages are being delivered or picked up."
```

## 🎯 Best Practices

### Performance Optimization
- **Model Selection**: Use faster models (gpt-4o-mini, gemini-1.5-flash) for real-time alerts
- **Token Limits**: Keep token counts reasonable (50-150) for faster responses
- **Frame Limits**: Limit video analysis to 3-5 frames for better performance

### Storage Management
- **Regular Cleanup**: Implement periodic cleanup of downloaded files
- **Directory Structure**: Use organized subdirectories by date or camera
- **Monitoring**: Track storage usage to prevent disk full issues

### Notification Strategy
- **Cooldown Periods**: Use 2-5 minute cooldowns to prevent spam
- **Zone Optimization**: Configure zones to reduce false positives
- **Device Grouping**: Group family devices for coordinated notifications

### AI Prompt Engineering
- **Be Specific**: Include relevant context about what you want to detect
- **Set Priorities**: Specify what events are most important
- **Include Context**: Mention zones, objects, and expected activities

## 🔧 Troubleshooting

### Common Issues

#### Notifications Not Received
- Verify Mobile App permissions for notifications
- Check device selection in automation configuration
- Ensure devices are online and connected

#### AI Analysis Failures
- Verify LLM Vision integration is properly configured
- Check API keys and provider status
- Review prompt complexity and token limits

#### File Download Errors
- Confirm Downloader integration is installed
- Verify directory permissions and available storage
- Check network connectivity to Frigate server

#### Event Detection Issues
- Review Frigate camera configuration and zones
- Check object detection settings and confidence levels
- Verify MQTT broker connectivity

For detailed troubleshooting, see the **[Troubleshooting Guide](docs/TROUBLESHOOTING.md)**.

## 🤝 Contributing

Contributions are welcome! Please feel free to:

- Report bugs and issues
- Suggest new features
- Submit pull requests
- Share configuration examples
- Improve documentation

## 📄 License

This project is licensed under the MIT License - see the repository for details.

## 🙏 Acknowledgments

- **[Frigate](https://github.com/blakeblackshear/frigate)** - Excellent NVR software
- **[LLM Vision](https://github.com/valentinfrlch/ha-llmvision)** - AI vision integration
- **[Home Assistant Community](https://community.home-assistant.io/)** - Ongoing support and inspiration

---

## 📚 Additional Resources

- [Frigate Documentation](https://docs.frigate.video/)
- [LLM Vision Integration Guide](https://github.com/valentinfrlch/ha-llmvision/wiki)
- [Home Assistant Automation Documentation](https://www.home-assistant.io/docs/automation/)
- [MQTT Configuration Guide](https://www.home-assistant.io/integrations/mqtt/)

**Version**: v0.41  
**Author**: whag  
**Last Updated**: January 2025
