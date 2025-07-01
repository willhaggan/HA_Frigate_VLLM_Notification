# Troubleshooting Guide - Frigate LLM Notification

This guide covers common issues and their solutions when using the Frigate LLM Notification automation.

## 🚨 Common Issues and Solutions

### 1. No Notifications Received

#### Symptoms
- Automation triggers but no mobile notifications arrive
- Events show in logs but devices don't receive alerts

#### Troubleshooting Steps

**Check Mobile App Configuration**
```yaml
# Verify these settings on your mobile device:
1. Home Assistant Companion App installed and logged in
2. Notification permissions enabled
3. Battery optimization disabled for HA app
4. Background activity allowed
```

**Verify Device Selection**
```yaml
# In automation configuration, ensure:
1. Correct mobile devices selected in "Notify Devices"
2. Device names match Home Assistant device registry
3. Integration: mobile_app filter is working
```

**Test Basic Notifications**
```yaml
# Create test automation:
- action: notify.mobile_app_your_device
  data:
    title: "Test Notification"
    message: "This is a test from Home Assistant"
```

**Check Device Entity Names**
```yaml
# Find correct device names:
Developer Tools → States → Search for "mobile_app"
# Look for entities like: notify.mobile_app_johns_phone
```

#### Common Solutions
- **Restart mobile app** and re-enable notifications
- **Check Do Not Disturb** settings on mobile device
- **Verify network connectivity** between HA and mobile device
- **Update Companion App** to latest version

### 2. AI Analysis Not Working

#### Symptoms
- Notifications use fallback messages instead of AI descriptions
- "LLM Vision analysis failed" in logs
- Blank or error messages in notifications

#### Troubleshooting Steps

**Verify LLM Vision Integration**
```yaml
# Check integration status:
Settings → Integrations → LLM Vision
# Ensure status is "Connected" and no error messages
```

**Test AI Provider Connection**
```yaml
# Test with simple prompt:
Developer Tools → Services
Service: llmvision.image_analyzer
Data:
  provider: your_provider
  model: your_model
  message: "Describe this image"
  image_url: "https://example.com/test-image.jpg"
```

**Check API Keys and Credits**
```yaml
# Verify:
1. API key is valid and not expired
2. Provider account has available credits/quota
3. Model name is correctly spelled
4. Provider service is operational
```

**Review Prompt Complexity**
```yaml
# Simplify prompt for testing:
Simple Test Prompt: "Describe what you see in this image in one sentence."

# Check for issues:
- Prompt too long (reduce complexity)
- Invalid template variables
- Special characters causing parsing errors
```

#### Common Solutions
- **Regenerate API keys** if authentication fails
- **Reduce prompt complexity** and token limits
- **Switch to different model** (e.g., gpt-4o-mini)
- **Check provider status pages** for service outages

### 3. File Download Errors

#### Symptoms
- "Failed to download image/clip" in logs
- Local file paths return 404 errors
- Automation stops at download step

#### Troubleshooting Steps

**Check Downloader Integration**
```yaml
# Verify downloader is configured:
configuration.yaml:
  downloader:
    download_dir: downloads  # or your custom path
```

**Verify Directory Permissions**
```yaml
# Check Home Assistant has write access:
# SSH or terminal access required
ls -la /config/downloads/
# Should show proper ownership and write permissions
```

**Test Download Manually**
```yaml
# Test download service:
Developer Tools → Services
Service: downloader.download_file
Data:
  url: "http://your-ha-ip:8123/api/frigate/notifications/123/snapshot.jpg"
  filename: "test_snapshot.jpg"
```

**Verify Network Connectivity**
```yaml
# Check Home Assistant can reach itself:
# Test URL format: http://192.168.1.100:8123/api/frigate/...
# Ensure:
1. IP address is correct in automation config
2. Port 8123 is accessible
3. No firewall blocking internal requests
```

#### Common Solutions
- **Fix directory permissions**: `chmod 755 /config/downloads`
- **Create download directory**: `mkdir -p /config/downloads`
- **Update Home Assistant IP** in automation configuration
- **Check available disk space**

### 4. Frigate Event Detection Issues

#### Symptoms
- Automation never triggers despite camera motion
- Wrong objects/zones trigger automation
- Events trigger for excluded areas

#### Troubleshooting Steps

**Verify Frigate MQTT Events**
```yaml
# Monitor MQTT messages:
Developer Tools → Events → Listen to Events
Event Type: mqtt_message_received

# Or check MQTT directly:
Topic: frigate/reviews
```

**Check Camera Configuration**
```yaml
# Verify in automation:
1. Correct camera selected from dropdown
2. Camera name matches Frigate configuration
3. Zones are properly defined in Frigate
4. Object detection is working in Frigate
```

**Test Zone Logic**
```yaml
# Check zone configuration:
- All Zones: false (any zone triggers)
- All Zones: true (all zones must trigger)

# Verify zone names match Frigate configuration
```

**Review Object Detection**
```yaml
# In Frigate web interface:
1. Check camera feed shows detection boxes
2. Verify objects are being detected consistently
3. Check confidence thresholds in Frigate config
```

#### Common Solutions
- **Restart Frigate** to refresh MQTT connections
- **Check MQTT broker** connectivity
- **Review Frigate camera config** for proper zones/objects
- **Adjust detection sensitivity** in Frigate

### 5. Performance Issues

#### Symptoms
- Very slow notification delivery
- High CPU usage during events
- Automation times out frequently

#### Troubleshooting Steps

**Optimize AI Model Selection**
```yaml
# Use faster models:
Fast Models:
  - gpt-4o-mini
  - gemini-1.5-flash
  - claude-3-5-haiku-latest

# Reduce complexity:
Tokens: 50-100 (instead of 200-300)
Max Frames: 2-3 (instead of 5-10)
```

**Check System Resources**
```yaml
# Monitor Home Assistant:
Settings → System → Hardware
# Look for:
- High CPU usage
- Low available RAM
- Disk I/O issues
```

**Optimize Download Settings**
```yaml
# Reduce wait times:
Image Download Max Wait Time: 3 (instead of 5)
Clip Download Max Wait Time: 15 (instead of 30)

# Reduce image size:
Pixels: 720 (instead of 1080)
```

#### Common Solutions
- **Use faster AI models** for real-time alerts
- **Reduce token limits** and frame analysis
- **Increase automation timeout** values
- **Optimize Frigate recording settings**

## 🔍 Debugging Techniques

### Enable Debug Logging
```yaml
# Add to configuration.yaml:
logger:
  default: info
  logs:
    homeassistant.components.automation: debug
    custom_components.llmvision: debug
    homeassistant.components.downloader: debug
    homeassistant.components.mqtt: debug
```

### Monitor Automation Execution
```yaml
# Check automation traces:
Settings → Automations & Scenes → [Your Automation] → Traces
# Look for:
- Where automation stops/fails
- Variable values at each step
- Condition evaluation results
```

### Test Individual Components
```yaml
# Test each component separately:

# 1. Test Frigate events
Developer Tools → Events → mqtt_message_received

# 2. Test LLM Vision
Developer Tools → Services → llmvision.image_analyzer

# 3. Test Downloads
Developer Tools → Services → downloader.download_file

# 4. Test Notifications
Developer Tools → Services → notify.mobile_app_device
```

## 📊 Log Analysis

### Key Log Messages

**Successful Operation**
```
INFO: LLM Vision Frigate Notification log
INFO: Event type: new
INFO: Camera triggered: front_door
INFO: Objects found: ['person']
INFO: Zone match: True
INFO: Notify devices: ['mobile_app_phone']
```

**Common Error Patterns**
```
# AI Analysis Errors
ERROR: llmvision failed to analyze image
ERROR: Invalid API key for provider

# Download Errors  
ERROR: Failed to download file from URL
ERROR: Permission denied writing to downloads directory

# Notification Errors
ERROR: mobile_app_device not found
ERROR: Failed to send notification to device
```

### Template Testing
```yaml
# Test template variables in Developer Tools:
Template Editor:
{{ camera_name }}                    # Should show camera name
{{ objects }}                       # Should show detected objects
{{ zone_names }}                    # Should show configured zones
{{ notify_names }}                  # Should show device names
```

## ⚡ Quick Fixes Checklist

### When Nothing Works
1. **Restart Home Assistant** - Solves many integration issues
2. **Check all integrations** - Ensure Frigate, LLM Vision, Downloader are working
3. **Verify MQTT broker** - Essential for Frigate communication
4. **Test with simple config** - Remove complexity to isolate issues

### When Notifications Fail
1. **Test basic notification** - Send test message to device
2. **Check device entity names** - Ensure correct device selection
3. **Verify app permissions** - Mobile app notification settings
4. **Check network connectivity** - Device connected to network

### When AI Analysis Fails
1. **Test API connection** - Use simple LLM Vision service call
2. **Check API credits/quota** - Ensure provider account is active
3. **Simplify prompts** - Use basic "describe image" prompt
4. **Try different model** - Switch to known working model

### When Files Don't Download
1. **Check directory exists** - Create downloads folder if missing
2. **Verify permissions** - Ensure Home Assistant can write files
3. **Test URL access** - Check Home Assistant IP configuration
4. **Check disk space** - Ensure adequate storage available

## 🆘 Getting Additional Help

### Community Resources
- **[Home Assistant Community](https://community.home-assistant.io/)** - Search for similar issues
- **[Frigate Discussions](https://github.com/blakeblackshear/frigate/discussions)** - Frigate-specific help
- **[LLM Vision Issues](https://github.com/valentinfrlch/ha-llmvision/issues)** - AI integration problems

### Information to Provide When Asking for Help
1. **Home Assistant version**
2. **Integration versions** (Frigate, LLM Vision)
3. **Error messages** from logs
4. **Automation configuration** (sanitized)
5. **What you've already tried**

### Creating Debug Reports
```yaml
# Collect this information:
1. Automation trace showing failure point
2. Relevant log entries with timestamps
3. Template test results
4. Integration status screenshots
5. Network/system resource information
```

---

**Remember**: Most issues are configuration-related. Start with simple setups and gradually add complexity once basic functionality is confirmed working.
