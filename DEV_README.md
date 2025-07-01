# 🚧 Frigate LLM Notification - Development Branch

[![Development](https://img.shields.io/badge/Status-Development-orange.svg)](https://github.com/willhaggan/HA_Frigate_VLLM_Notification/tree/dev)
[![Version](https://img.shields.io/badge/Version-v0.42--dev-red.svg)](https://github.com/willhaggan/HA_Frigate_VLLM_Notification/blob/dev/Dev.yaml)
[![Stability](https://img.shields.io/badge/Stability-Experimental-red.svg)](https://github.com/willhaggan/HA_Frigate_VLLM_Notification/tree/dev)

**⚠️ WARNING: This is the development branch containing experimental features. Use at your own risk!**

For the stable version, please use the [main branch](https://github.com/willhaggan/HA_Frigate_VLLM_Notification) and `Latest.yaml`.

## 🆕 Development Features

This development branch includes experimental features and improvements that are being tested before inclusion in the stable release:

### 🐛 Enhanced Debugging & Logging
- **Debug Log Levels**: Configurable logging verbosity (ERROR, WARNING, INFO, DEBUG)
- **Performance Monitoring**: Track operation timings and performance metrics
- **Enhanced Error Messages**: More detailed error reporting and troubleshooting information
- **Retry Logic**: Configurable retry attempts for failed operations

### 🚀 Performance Improvements
- **Timeout Configuration**: Adjustable timeout durations for operations
- **Enhanced Retry Mechanisms**: Improved resilience for network and API failures
- **Optimized Processing**: Better handling of concurrent events
- **Resource Management**: Improved memory and CPU usage patterns

### 🤖 Extended AI Model Support
New AI models added for testing:
- **GPT Models**: `gpt-5-preview` (when available)
- **Gemini Models**: `gemini-3.0-preview` (experimental)
- **Llama Models**: `llama-3.3-70b-versatile`
- **Open Source Models**: 
  - `qwen2-vl-72b-instruct`
  - `deepseek-vl-7b-chat`
  - `yi-vision-plus`

### 🔧 Advanced Configuration Options
- **Experimental Features Toggle**: Enable/disable experimental functionality
- **Performance Tracking**: Monitor automation performance
- **Enhanced Debugging**: Detailed logging and troubleshooting tools
- **Timeout Management**: Configurable operation timeouts

### 📱 Interactive Notification Action Buttons (New!)
Advanced notification action buttons with customizable functionality:
- **Up to 3 Action Buttons**: Fully customizable button text and actions
- **Smart Icons**: Automatic icon assignment based on action type
- **Multiple Action Types**:
  - View Live Camera Feed
  - Download Snapshot/Clip
  - Mark Event as Reviewed
  - Silence Notifications (30min/1hour)
  - Toggle Camera Detection
  - Run Custom Automations
  - Emergency Protocol Activation
  - Share Clips to Telegram
  - Custom Service Calls
- **Enhanced Security**: Authentication required for sensitive actions
- **Visual Styling**: Multiple button styles and timeout options
- **Persistent Actions**: Keep buttons active after notification dismissal

## 📋 Development Configuration

### New Input Options

```yaml
# Debug Section (New in Dev)
Debug:
  enable_debug_logs: true/false
  debug_log_level: ERROR|WARNING|INFO|DEBUG
  retry_attempts: 1-10 (default: 3)
  timeout_duration: 30-300 seconds (default: 60)
  experimental_features: true/false
  performance_monitoring: true/false

# Notification Action Buttons (New in Dev)
Notification Options:
  enable_action_buttons: true/false
  
  # Button 1 Configuration
  action_button_1_text: "View Live"
  action_button_1_action: view_live_camera/download_snapshot/etc.
  action_button_1_data: "media_player.living_room" # Optional data
  
  # Button 2 Configuration  
  action_button_2_text: "Dismiss"
  action_button_2_action: dismiss_notification/toggle_camera/etc.
  action_button_2_data: "switch.camera_detect" # Optional data
  
  # Button 3 Configuration (Optional)
  action_button_3_text: "Share"
  action_button_3_action: share_clip/emergency_protocol/etc.
  action_button_3_data: "" # Optional data
  
  # Advanced Button Settings
  action_button_style: default/colored/icons_only/text_only/large
  action_timeout: 30 # Minutes (0 = no timeout)
  persistent_actions: true/false
```

### Development Variables

New template variables available:
- `{{debug_enabled}}` - Debug logging status
- `{{debug_level}}` - Current debug level
- `{{retry_count}}` - Number of retry attempts configured
- `{{operation_timeout}}` - Timeout duration setting
- `{{experimental_enabled}}` - Experimental features status
- `{{performance_tracking}}` - Performance monitoring status

## 🧪 Testing Features

### Debug Logging Example
```yaml
# Enable debug logging for troubleshooting
Debug Options:
  Enable Debug Logging: true
  Debug Log Level: DEBUG
  Performance Monitoring: true
```

### Enhanced Error Handling
```yaml
# Configure resilience settings
Debug Options:
  Enhanced Retry Attempts: 5
  Operation Timeout: 120
  Experimental Features: true
```

### AI Model Testing
```yaml
# Test new AI models
LLM Vision Provider:
  Model: gemini-3.0-preview  # Experimental model
  # or
  Model: qwen2-vl-72b-instruct  # Open source alternative
```

## 🚨 Known Issues & Limitations

### Development Warnings
- **Stability**: Experimental features may cause unexpected behavior
- **Performance**: Debug logging can impact performance
- **Compatibility**: New models may not be available with all providers
- **Support**: Limited support for development features

### Testing Recommendations
1. **Start Small**: Test with one camera and basic settings
2. **Monitor Logs**: Enable debug logging to track behavior
3. **Backup Configuration**: Keep stable automation backup
4. **Report Issues**: Document any problems encountered

## 📊 Performance Monitoring

When enabled, performance monitoring tracks:
- **LLM Response Times**: AI analysis duration
- **Download Speeds**: File download performance
- **Processing Delays**: Total automation execution time
- **Error Rates**: Failure frequency and types

## 🔄 Changelog - Development Branch

### v0.42-dev (Current)
- ✅ Added comprehensive debugging options
- ✅ Enhanced error handling and retry logic
- ✅ Extended AI model support (experimental models)
- ✅ Performance monitoring capabilities
- ✅ Configurable timeout management
- ✅ Improved logging and troubleshooting

### Planned Features
- 🔄 Multi-camera coordination improvements
- 🔄 Advanced notification formatting
- 🔄 Integration with additional AI providers
- 🔄 Enhanced file management options
- 🔄 Real-time performance dashboard

## 🛠️ Development Setup

### Installation (Development Branch)
1. **Import Development Blueprint**:
   ```
   https://github.com/willhaggan/HA_Frigate_VLLM_Notification/blob/dev/Dev.yaml
   ```

2. **Enable Development Features**:
   - Set "Enable Debug Logging" to `true`
   - Choose appropriate "Debug Log Level"
   - Enable "Experimental Features" if desired

3. **Monitor Performance**:
   - Enable "Performance Monitoring"
   - Check Home Assistant logs for detailed information

### Upgrading from Stable
1. **Backup** your current automation configuration
2. **Test** the development version alongside stable version
3. **Monitor** logs for any issues
4. **Report** feedback on GitHub issues

## 📝 Feedback & Bug Reports

### How to Report Issues
1. **Enable Debug Logging** (`DEBUG` level)
2. **Reproduce the Issue** and capture logs
3. **Create GitHub Issue** with:
   - Debug logs
   - Configuration details
   - Expected vs actual behavior
   - Environment information

### Feature Requests
- **GitHub Discussions**: For feature ideas and feedback
- **GitHub Issues**: For specific feature requests
- **Community Forum**: For general discussion

## ⚠️ Migration Path

### From Development to Stable
When features are proven stable, they will be:
1. **Refined** based on testing feedback
2. **Documented** with complete examples
3. **Merged** into the main branch
4. **Released** as part of the next stable version

### Staying Updated
- **Watch** the repository for updates
- **Test** new development features
- **Provide Feedback** on your experience
- **Contribute** improvements and bug fixes

---

## 🔗 Links

- **[Stable Version](https://github.com/willhaggan/HA_Frigate_VLLM_Notification)** - Main branch with Latest.yaml
- **[Documentation](../docs/)** - Complete documentation (applies to both versions)
- **[Issues](https://github.com/willhaggan/HA_Frigate_VLLM_Notification/issues)** - Bug reports and feature requests
- **[Discussions](https://github.com/willhaggan/HA_Frigate_VLLM_Notification/discussions)** - Community discussion

**Remember**: This is experimental software. Always test thoroughly and maintain backups of your working configuration!

---

# 🚀 Frigate LLM Notification - Action Button Configuration Guide

This guide provides comprehensive information on configuring and using the new action button features in the Frigate LLM Notification system. Action buttons allow for interactive notifications with customizable actions, enhancing the capabilities of your home automation setup.

## 📋 Action Button Configuration

### New Configuration Options

```yaml
# Notification Action Buttons
Notification Options:
  enable_action_buttons: true/false
  
  # Button 1 Configuration
  action_button_1_text: "View Live"
  action_button_1_action: view_live_camera/download_snapshot/etc.
  action_button_1_data: "media_player.living_room" # Optional data
  
  # Button 2 Configuration  
  action_button_2_text: "Dismiss"
  action_button_2_action: dismiss_notification/toggle_camera/etc.
  action_button_2_data: "switch.camera_detect" # Optional data
  
  # Button 3 Configuration (Optional)
  action_button_3_text: "Share"
  action_button_3_action: share_clip/emergency_protocol/etc.
  action_button_3_data: "" # Optional data
  
  # Advanced Button Settings
  action_button_style: default/colored/icons_only/text_only/large
  action_timeout: 30 # Minutes (0 = no timeout)
  persistent_actions: true/false
```

### 🔒 Security Features

- **Authentication Required**: Sensitive actions require device authentication
- **Destructive Action Warnings**: Visual indicators for potentially harmful actions
- **Timeout Protection**: Buttons automatically expire after configured time
- **Action Logging**: All button presses logged to Home Assistant logbook

## 📋 Configuration Examples

### 🎯 Basic Action Button Setup

```yaml
# Enable action buttons
enable_action_buttons: true

# Simple configuration for common use cases
action_button_1_text: "📹 View Live"
action_button_1_action: view_live_camera
action_button_1_data: ""

action_button_2_text: "❌ Dismiss"
action_button_2_action: dismiss_notification
action_button_2_data: ""

action_button_3_text: ""
action_button_3_action: ""
action_button_3_data: ""
```

### 🏠 Home Security Setup

```yaml
# Security-focused configuration
enable_action_buttons: true

action_button_1_text: "🚨 View Live Camera"
action_button_1_action: view_live_camera
action_button_1_data: "media_player.living_room_tv"  # Cast to TV

action_button_2_text: "🔒 Arm Security"
action_button_2_action: toggle_security
action_button_2_data: "alarm_control_panel.house_alarm"  # Security system

action_button_3_text: "🚔 Emergency"
action_button_3_action: emergency_protocol
action_button_3_data: "script.emergency_response"  # Emergency script

# Advanced settings
action_button_style: colored
action_timeout: 60  # 1 hour timeout
persistent_actions: true
```

### 🎬 Media Management Setup

```yaml
# Media-focused configuration
enable_action_buttons: true

action_button_1_text: "💾 Download Clip"
action_button_1_action: download_clip
action_button_1_data: "security_{{camera}}_{{id}}.mp4"  # Custom filename

action_button_2_text: "📤 Share Clip"
action_button_2_action: share_clip
action_button_2_data: ""  # Will use default sharing method

action_button_3_text: "🗂️ Archive"
action_button_3_action: backup_clip
action_button_3_data: "/backup/security/{{camera}}/{{id}}.mp4"  # Backup path

action_button_style: large
action_timeout: 120  # 2 hours
```

### 🏢 Business/Commercial Setup

```yaml
# Professional monitoring setup
enable_action_buttons: true

action_button_1_text: "👁️ Dashboard"
action_button_1_action: view_frigate_dashboard
action_button_1_data: "https://frigate.company.com/cameras/{{camera}}"

action_button_2_text: "📞 Security Team"
action_button_2_action: send_security
action_button_2_data: "notify.security_team_telegram"  # Telegram group

action_button_3_text: "📋 Create Report"
action_button_3_action: run_automation
action_button_3_data: "automation.create_security_incident_report"

action_button_style: text_only
action_timeout: 30
persistent_actions: false
```

### 🔧 Advanced Custom Setup

```yaml
# Power user configuration with custom services
enable_action_buttons: true

action_button_1_text: "🔍 Analyze"
action_button_1_action: custom_service
action_button_1_data: "script.ai_analysis|{{image_local}}"  # Service|parameter

action_button_2_text: "🔕 Silence 1hr"
action_button_2_action: silence_1hour
action_button_2_data: "input_boolean.frigate_{{camera}}_notifications"

action_button_3_text: "🌐 Open URL"
action_button_3_action: custom_url
action_button_3_data: "https://my-dashboard.com/camera/{{camera}}/event/{{id}}"

action_button_style: icons_only
action_timeout: 0  # No timeout
persistent_actions: true
```

## 🎛️ Action Button Reference Guide

### 📱 Standard Actions (No Data Required)

```yaml
# Simple actions that don't need additional data
action_button_1_action: view_live_camera      # Opens camera stream
action_button_1_action: download_snapshot     # Downloads image
action_button_1_action: download_clip         # Downloads video
action_button_1_action: mark_reviewed         # Marks event as seen
action_button_1_action: silence_30min         # Silences for 30 minutes
action_button_1_action: silence_1hour         # Silences for 1 hour
action_button_1_action: dismiss_notification  # Clears notification
action_button_1_action: share_clip           # Shares via default method
action_button_1_action: add_timeline         # Adds to timeline
```

### 🎯 Entity-Based Actions (Require Entity Data)

```yaml
# Actions that control Home Assistant entities
action_button_1_action: toggle_camera
action_button_1_data: "switch.front_door_camera_detect"

action_button_1_action: toggle_security
action_button_1_data: "alarm_control_panel.house_alarm"

action_button_1_action: run_automation
action_button_1_data: "automation.lights_on_motion"

action_button_1_action: custom_service
action_button_1_data: "light.turn_on|light.front_porch"  # service|entity
```

### 🌐 URL-Based Actions

```yaml
# Actions that open URLs or custom interfaces
action_button_1_action: custom_url
action_button_1_data: "https://frigate.local:5000/cameras/{{camera}}"

action_button_1_action: view_frigate_dashboard
action_button_1_data: "https://my-frigate.com/review/{{review_id}}"

action_button_1_action: view_camera_settings
action_button_1_data: "https://camera-config.local/{{camera}}"
```

### 📁 File-Based Actions (Custom Paths)

```yaml
# Actions that work with files and paths
action_button_1_action: download_snapshot
action_button_1_data: "security_{{camera}}_{{timestamp}}.jpg"

action_button_1_action: download_clip
action_button_1_data: "clips/{{camera}}/{{date}}/{{id}}.mp4"

action_button_1_action: backup_clip
action_button_1_data: "/nas/security/archive/{{camera}}/{{id}}.mp4"
```

### 📨 Notification Actions

```yaml
# Actions for advanced notification handling
action_button_1_action: send_security
action_button_1_data: "notify.telegram_security_group"

action_button_1_action: mark_false_positive
action_button_1_data: "input_boolean.false_positive_{{id}}"

action_button_1_action: view_similar
action_button_1_data: "{{objects[0]}}"  # Search by detected object
```

## 🔤 Template Variables Available

When configuring data fields, you can use these template variables:

```yaml
# Event Information
{{id}}           # Detection ID
{{review_id}}    # Review ID  
{{camera}}       # Camera name
{{objects}}      # Detected objects list
{{type}}         # Event type (new/end)
{{timestamp}}    # Event timestamp

# File Paths
{{image_local}}  # Local snapshot path
{{video_local}}  # Local clip path
{{image}}        # Frigate snapshot URL
{{video}}        # Frigate clip URL
{{gif}}          # Preview GIF URL

# Date/Time
{{date}}         # Current date (YYYY-MM-DD)
{{time}}         # Current time (HH:MM:SS)
{{datetime}}     # Full datetime

# Camera Info
{{camera_name}}  # Friendly camera name
{{zone}}         # Triggered zone
{{severity}}     # Alert severity
```

## 🎨 Button Style Examples

```yaml
# Different visual styles
action_button_style: default      # Standard iOS style
action_button_style: colored      # Colored buttons with theme
action_button_style: icons_only   # Only show icons
action_button_style: text_only    # Only show text
action_button_style: large        # Larger button size
```

## ⏰ Timeout Configuration Examples

```yaml
# Timeout settings
action_timeout: 0      # No timeout (buttons stay active)
action_timeout: 15     # 15 minutes
action_timeout: 30     # 30 minutes (default)
action_timeout: 60     # 1 hour
action_timeout: 240    # 4 hours
action_timeout: 1440   # 24 hours (maximum)

# Persistent actions
persistent_actions: true   # Buttons remain after dismissing notification
persistent_actions: false  # Buttons disappear with notification (default)
```

## 🔄 Real-World Usage Scenarios

### 🏠 Doorbell Camera
```yaml
action_button_1_text: "🔔 Answer Door"
action_button_1_action: custom_service
action_button_1_data: "script.answer_door|front_door"

action_button_2_text: "🔒 Lock Door"
action_button_2_action: custom_service
action_button_2_data: "lock.lock|lock.front_door_smart_lock"

action_button_3_text: "💬 Speak Message"
action_button_3_action: run_automation
action_button_3_data: "automation.doorbell_custom_message"
```

### 🚗 Driveway Monitoring
```yaml
action_button_1_text: "🚪 Open Garage"
action_button_1_action: custom_service
action_button_1_data: "cover.open_cover|cover.garage_door"

action_button_2_text: "💡 Turn On Lights"
action_button_2_action: custom_service
action_button_2_data: "light.turn_on|light.driveway_lights"

action_button_3_text: "📝 Log Vehicle"
action_button_3_action: run_automation
action_button_3_data: "automation.log_vehicle_entry"
```

### 🌙 Night Security
```yaml
action_button_1_text: "🔦 Spotlight On"
action_button_1_action: custom_service
action_button_1_data: "light.turn_on|light.security_spotlight"

action_button_2_text: "📢 Play Warning"
action_button_2_action: run_automation
action_button_2_data: "automation.security_warning_announcement"

action_button_3_text: "📱 Call Security"
action_button_3_action: emergency_protocol
action_button_3_data: "script.contact_security_company"
```

## 📚 Additional Resources

### 📖 Comprehensive Documentation
- **[📱 Action Button Examples](docs/ACTION_BUTTON_EXAMPLES.md)** - Detailed configuration examples for all scenarios
- **[🚀 Quick Start Guide](docs/QUICK_START.md)** - Get started quickly with the blueprint
- **[⚙️ Configuration Guide](docs/CONFIGURATION.md)** - Complete configuration reference
- **[🔧 Troubleshooting](docs/TROUBLESHOOTING.md)** - Common issues and solutions

### 🎯 Example Files
The `docs/ACTION_BUTTON_EXAMPLES.md` file contains:
- **50+ real-world configuration examples**
- **Complete template variable reference**
- **Security and business use cases**
- **IoT and smart home integrations**
- **Debugging and testing guides**

### 🔗 Quick Links
- [Stable Version (Latest.yaml)](Latest.yaml) - Production-ready blueprint
- [Development Version (Dev.yaml)](Dev.yaml) - Latest experimental features
- [GitHub Repository](https://github.com/willhaggan/HA_Frigate_VLLM_Notification) - Source code and issues

---

**⚠️ Remember**: This is a development branch with experimental features. Test thoroughly before using in production environments!
