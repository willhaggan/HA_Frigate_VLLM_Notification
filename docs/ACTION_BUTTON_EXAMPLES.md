# 📱 Action Button Examples - Dev Branch

This document provides comprehensive examples for configuring the new interactive notification action buttons in the development branch of the Frigate LLM Notification blueprint.

## 🎯 Quick Start Examples

### Basic Three-Button Setup
```yaml
# Notification Options
enable_action_buttons: true

# Button 1: View Live Camera
action_button_1_text: "📹 View Live"
action_button_1_action: view_live_camera
action_button_1_data: ""

# Button 2: Download Snapshot
action_button_2_text: "📸 Save Image"
action_button_2_action: download_snapshot
action_button_2_data: "{{camera}}_{{id}}.jpg"

# Button 3: Dismiss
action_button_3_text: "❌ Dismiss"
action_button_3_action: dismiss_notification
action_button_3_data: ""

# Style Settings
action_button_style: default
action_timeout: 30
persistent_actions: false
```

## 🏠 Home Security Scenarios

### 🚪 Front Door Monitoring
```yaml
enable_action_buttons: true

# Button 1: View live feed on living room TV
action_button_1_text: "📺 View on TV"
action_button_1_action: view_live_camera
action_button_1_data: "media_player.living_room_chromecast"

# Button 2: Unlock door remotely
action_button_2_text: "🔓 Unlock Door"
action_button_2_action: custom_service
action_button_2_data: "lock.unlock|lock.front_door_smart_lock"

# Button 3: Speak to visitor
action_button_3_text: "🎤 Speak"
action_button_3_action: run_automation
action_button_3_data: "automation.doorbell_intercom_mode"

action_button_style: large
action_timeout: 120
persistent_actions: true
```

### 🌙 Night Security Alert
```yaml
enable_action_buttons: true

# Button 1: Turn on all exterior lights
action_button_1_text: "💡 Lights On"
action_button_1_action: custom_service
action_button_1_data: "homeassistant.turn_on|group.exterior_lights"

# Button 2: Arm all security systems
action_button_2_text: "🔒 Full Arm"
action_button_2_action: toggle_security
action_button_2_data: "alarm_control_panel.house_security"

# Button 3: Emergency response
action_button_3_text: "🚨 Emergency"
action_button_3_action: emergency_protocol
action_button_3_data: "script.security_emergency_protocol"

action_button_style: colored
action_timeout: 60
persistent_actions: true
```

### 🚗 Driveway & Garage
```yaml
enable_action_buttons: true

# Button 1: Open garage door
action_button_1_text: "🚪 Open Garage"
action_button_1_action: custom_service
action_button_1_data: "cover.open_cover|cover.main_garage_door"

# Button 2: Turn on driveway lights
action_button_2_text: "🔦 Driveway Lights"
action_button_2_action: custom_service
action_button_2_data: "light.turn_on|light.driveway_motion_lights"

# Button 3: Log vehicle entry
action_button_3_text: "📝 Log Entry"
action_button_3_action: run_automation
action_button_3_data: "automation.vehicle_entry_logger"

action_button_style: text_only
action_timeout: 45
persistent_actions: false
```

## 🏢 Business & Commercial Examples

### 🏪 Retail Store Security
```yaml
enable_action_buttons: true

# Button 1: View on security monitor
action_button_1_text: "🖥️ Security Monitor"
action_button_1_action: view_live_camera
action_button_1_data: "media_player.security_office_display"

# Button 2: Alert security team
action_button_2_text: "👮 Alert Security"
action_button_2_action: send_security
action_button_2_data: "notify.security_team_telegram"

# Button 3: Create incident report
action_button_3_text: "📋 Incident Report"
action_button_3_action: run_automation
action_button_3_data: "automation.create_security_incident"

action_button_style: icons_only
action_timeout: 0  # No timeout for business use
persistent_actions: true
```

### 🏭 Warehouse Monitoring
```yaml
enable_action_buttons: true

# Button 1: View full warehouse dashboard
action_button_1_text: "📊 Dashboard"
action_button_1_action: custom_url
action_button_1_data: "https://warehouse-security.company.com/cameras/{{camera}}"

# Button 2: Stop conveyor belt (emergency)
action_button_2_text: "⏹️ Emergency Stop"
action_button_2_action: custom_service
action_button_2_data: "switch.turn_off|switch.main_conveyor_belt"

# Button 3: Page supervisor
action_button_3_text: "📞 Page Supervisor"
action_button_3_action: run_automation
action_button_3_data: "automation.page_warehouse_supervisor"

action_button_style: large
action_timeout: 15  # Quick response needed
persistent_actions: true
```

## 🎬 Media & Content Management

### 📹 Content Creator Setup
```yaml
enable_action_buttons: true

# Button 1: Download high-quality clip
action_button_1_text: "🎬 Download HD"
action_button_1_action: download_clip
action_button_1_data: "content/{{camera}}/{{date}}/{{id}}_HD.mp4"

# Button 2: Share to social media
action_button_2_text: "📤 Share Social"
action_button_2_action: share_clip
action_button_2_data: "automation.social_media_post"

# Button 3: Add to highlight reel
action_button_3_text: "⭐ Add to Reel"
action_button_3_action: add_timeline
action_button_3_data: "input_boolean.highlight_reel_{{camera}}"

action_button_style: colored
action_timeout: 180  # 3 hours for content review
persistent_actions: true
```

### 📚 Educational/Training Use
```yaml
enable_action_buttons: true

# Button 1: Save as training example
action_button_1_text: "🎓 Training Example"
action_button_1_action: backup_clip
action_button_1_data: "/training/examples/{{objects[0]}}/{{id}}.mp4"

# Button 2: Annotate for AI training
action_button_2_text: "🏷️ Annotate"
action_button_2_action: custom_url
action_button_2_data: "https://annotation-tool.com/annotate/{{id}}"

# Button 3: Share with team
action_button_3_text: "👥 Share Team"
action_button_3_action: send_security
action_button_3_data: "notify.training_team_slack"

action_button_style: default
action_timeout: 240  # 4 hours for review
persistent_actions: false
```

## 🔧 Advanced Custom Integrations

### 🤖 Smart Home Automation
```yaml
enable_action_buttons: true

# Button 1: Run AI analysis script
action_button_1_text: "🧠 AI Analysis"
action_button_1_action: custom_service
action_button_1_data: "script.advanced_ai_analysis|{{image_local}}"

# Button 2: Trigger scene based on detection
action_button_2_text: "🎭 Smart Scene"
action_button_2_action: run_automation
action_button_2_data: "automation.smart_scene_{{objects[0]}}"

# Button 3: Update machine learning model
action_button_3_text: "📈 Update ML"
action_button_3_action: custom_service
action_button_3_data: "python_script.update_ml_model|{{camera}},{{objects}}"

action_button_style: text_only
action_timeout: 60
persistent_actions: false
```

### 🌐 IoT Integration
```yaml
enable_action_buttons: true

# Button 1: Send to MQTT broker
action_button_1_text: "📡 Send MQTT"
action_button_1_action: custom_service
action_button_1_data: "mqtt.publish|iot/security/{{camera}}/alert"

# Button 2: Update IoT dashboard
action_button_2_text: "📊 IoT Dashboard"
action_button_2_action: custom_url
action_button_2_data: "https://iot-dashboard.com/security/{{camera}}/{{id}}"

# Button 3: Trigger webhook
action_button_3_text: "🔗 Webhook"
action_button_3_action: custom_service
action_button_3_data: "rest_command.security_webhook|{{review_id}}"

action_button_style: icons_only
action_timeout: 30
persistent_actions: false
```

## 📊 Data Field Reference

### 🔤 Template Variables
```yaml
# Available in all data fields:
{{id}}           # Detection ID (e.g., "1704067200.123456-abc123")
{{review_id}}    # Review ID for the event
{{camera}}       # Camera name (e.g., "front_door")
{{camera_name}}  # Friendly camera name (e.g., "Front Door")
{{objects}}      # List of detected objects (e.g., ["person", "car"])
{{objects[0]}}   # First detected object
{{type}}         # Event type ("new" or "end")
{{timestamp}}    # Unix timestamp
{{date}}         # Date in YYYY-MM-DD format
{{time}}         # Time in HH:MM:SS format
{{zone}}         # Triggered zone name

# File-related variables:
{{image_local}}  # Local snapshot path
{{video_local}}  # Local clip path
{{image}}        # Frigate snapshot URL
{{video}}        # Frigate clip URL
{{gif}}          # Preview GIF URL
```

### 🎯 Entity Formats
```yaml
# For toggle_camera, toggle_security, custom_service:
action_button_X_data: "entity_id"
action_button_X_data: "switch.camera_detect"
action_button_X_data: "alarm_control_panel.house"

# For custom_service with parameters:
action_button_X_data: "service_name|entity_id"
action_button_X_data: "light.turn_on|light.front_porch"
action_button_X_data: "script.custom_script|parameter_value"

# For run_automation:
action_button_X_data: "automation.entity_id"
action_button_X_data: "automation.motion_response"
```

### 🌐 URL Formats
```yaml
# For custom_url, view_frigate_dashboard:
action_button_X_data: "https://full-url.com/path"
action_button_X_data: "https://frigate.local:5000/cameras/{{camera}}"
action_button_X_data: "https://dashboard.com/event/{{id}}"

# For local network addresses:
action_button_X_data: "http://192.168.1.100:8080/camera/{{camera}}"
```

### 📁 File Path Formats
```yaml
# For download_snapshot, download_clip, backup_clip:
action_button_X_data: "filename.ext"
action_button_X_data: "{{camera}}_{{id}}.jpg"
action_button_X_data: "/full/path/{{camera}}/{{date}}/{{id}}.mp4"
action_button_X_data: "security/{{objects[0]}}/{{timestamp}}.jpg"
```

## 🎨 Style & Behavior Options

### 🎭 Button Styles
```yaml
action_button_style: default      # Standard iOS notification style
action_button_style: colored      # Colored buttons with visual themes
action_button_style: icons_only   # Show only icons, no text
action_button_style: text_only    # Show only text, no icons
action_button_style: large        # Larger button size for accessibility
```

### ⏱️ Timeout Behavior
```yaml
action_timeout: 0      # Buttons never expire
action_timeout: 15     # Expire after 15 minutes
action_timeout: 30     # Expire after 30 minutes (default)
action_timeout: 60     # Expire after 1 hour
action_timeout: 1440   # Expire after 24 hours (maximum)
```

### 🔄 Persistence Settings
```yaml
persistent_actions: false  # Buttons disappear when notification dismissed (default)
persistent_actions: true   # Buttons remain available in notification history
```

## 🔒 Security Considerations

### 🛡️ Authentication Required Actions
These actions automatically require device authentication:
- `emergency_protocol`
- `toggle_security`
- `custom_service` (when modifying locks, alarms, or critical systems)
- `run_automation` (for security-related automations)
- `send_security`

### ⚠️ Destructive Actions
These actions show warning indicators:
- `emergency_protocol`
- `toggle_security`
- `silence_1hour`
- `mark_false_positive`
- Any custom service that could affect security

### 📝 Action Logging
All button presses are logged to the Home Assistant logbook with:
- User who pressed the button
- Action taken
- Timestamp
- Associated event ID
- Any errors or results

## 🧪 Testing Your Configuration

### 🔍 Debug Mode
Enable debug logging to see action button processing:
```yaml
enable_debug_logs: true
debug_log_level: DEBUG
```

### 📋 Validation Checklist
- [ ] Button text is clear and descriptive
- [ ] Actions match your intended functionality
- [ ] Data fields use correct entity IDs or URLs
- [ ] Template variables are properly formatted
- [ ] Timeout values are appropriate for your use case
- [ ] Security settings match your requirements

### 🎯 Common Issues
1. **Entity not found**: Check entity IDs in Developer Tools
2. **URL not opening**: Verify URLs are accessible from mobile device
3. **Service not executing**: Ensure services exist and have proper permissions
4. **Templates not working**: Check template syntax in Template Editor

---

💡 **Tip**: Start with simple configurations and gradually add complexity as you become familiar with the action button system.

🔗 **Need Help?** Check the main DEV_README.md for additional documentation and troubleshooting tips.
