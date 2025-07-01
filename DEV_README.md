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
