# Frigate LLM Notification v0.6 - Intelligent Security Camera Automation

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2024.1%2B-blue.svg)](https://www.home-assistant.io/)
[![Frigate](https://img.shields.io/badge/Frigate-0.13%2B-green.svg)](https://github.com/blakeblackshear/frigate)
[![LLM Vision](https://img.shields.io/badge/LLM%20Vision-Integration-orange.svg)](https://github.com/valentinfrlch/ha-llmvision)

An advanced Home Assistant automation blueprint that combines **Frigate NVR**, **LLM Vision AI**, and **interactive mobile notifications** to create intelligent, context-aware security alerts with actionable buttons.

## ⚠️ **IMPORTANT: Migration Notice for v0.6**

### **🚨 Breaking Changes - Manual Migration Required**

**Version 0.6 introduces significant structural changes that prevent automatic migration from previous versions.**

#### **If you are upgrading from v0.43/0.5-dev or earlier:**

1. **📝 Document your existing settings** - Note down all your current automation configurations
2. **🗑️ Delete existing automations** created with previous blueprint versions
3. **📥 Import the new v0.6 blueprint** (replaces the old blueprint)
4. **🆕 Create new automations** using the v0.6 blueprint with your documented settings
5. **🔧 Configure new button features** and filtering options

#### **Why manual migration is required:**
- **18+ new button configuration inputs** that didn't exist in previous versions
- **Enhanced filtering system** with new person/zone validation
- **Restructured template logic** for button event handling
- **New variable structure** for safe event processing

#### **Migration cannot be automated because:**
- New inputs have no equivalent in old versions
- Template logic has been fundamentally restructured
- Button functionality requires explicit user configuration
- Filtering options need individual setup decisions

---

## 🌟 Key Features

### 🎯 Interactive Notifications
- **📱 3 Customizable Buttons** per notification type (New Event + End Event)
- **🔇 Smart Silence Feature** with custom timer input (5-120 minutes)
- **📞 Direct Call Integration** with configurable phone numbers
- **📸 Quick Actions**: View Live, View Snapshot, View Clip
- **🏠 Custom Actions**: Execute any Home Assistant automation

### 🤖 Advanced AI Analysis
- **Dual Analysis**: Initial snapshot + comprehensive video clip analysis
- **Multiple LLM Support**: GPT-4, Claude, Gemini, Llama models
- **Person-Verified Detection**: Handles Frigate's enhanced person detection
- **Memory Integration**: Uses LLM Vision timeline and memory features
- **Custom Prompts**: Tailored AI analysis for your specific needs

### 🛡️ Smart Filtering System
- **Zone Vacancy Detection**: Only alert when specific people are away
- **Person Presence Validation**: Check if family members are home/away
- **Custom Conditions**: Add complex logic conditions
- **Object Requirements**: Require ALL specified objects (not just any)
- **Sub-label Support**: Advanced filtering with Frigate sub-labels

### 🔧 Features
- **Optional Downloader**: Choose between direct URLs or local file storage
- **Parallel Processing**: Handle multiple camera events simultaneously
- **Enhanced Error Handling**: Robust retry logic and safe template processing
- **Button Event Handling**: Proper event processing for all interaction types
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

### 🆕 **New Installation (v0.6)**

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
Button 1: "View Live" (VIEW_LIVE)
Button 2: "Silence" (SILENCE)
```

#### 3. Test Your Setup
1. **Create automation** with minimal settings above
2. **Trigger person detection** at your camera
3. **Verify notification** appears with working buttons
4. **Test silence functionality** with custom timer

---

### 🔄 **Migration from Previous Versions**

#### **Step 1: Document Current Settings**
Before making any changes, document your existing automations:

```yaml
# For each existing automation, note:
Camera: camera.your_camera
Objects: ["person", "car"]
Zones: ["front_porch", "driveway"] 
AI Prompts: "Your custom prompts"
Notification Devices: [your_devices]
Custom Actions: [any_custom_actions]
Downloader Settings: enabled/disabled
LLM Provider: your_provider
LLM Model: your_model
```

#### **Step 2: Remove Old Automations**
1. **Go to Settings → Automations & Scenes**
2. **Find automations** created with previous blueprint versions
3. **Document their settings** (see Step 1)
4. **Delete the old automations**

#### **Step 3: Import New Blueprint**
1. **Import v0.6 blueprint** using the URL above
2. **Old blueprint will be replaced** automatically

#### **Step 4: Recreate Automations**
1. **Create new automation** from v0.6 blueprint
2. **Apply your documented settings**
3. **Configure new button features**:
   ```yaml
   # New Event Buttons
   Button 1: "View Live" → VIEW_LIVE
   Button 2: "Silence" → SILENCE
   Button 3: "Call" → CALL (optional)
   
   # End Event Buttons  
   Button 1: "View Clip" → VIEW_CLIP
   Button 2: "View Live" → VIEW_LIVE
   Button 3: "Silence" → SILENCE
   ```
4. **Set up new filtering options** (optional):
   ```yaml
   # Advanced Filtering (Optional)
   Vacant Zone: zone.home
   Persons Away: ["person.john", "person.jane"]
   Custom Condition: "{{ now().hour >= 22 }}"  # Example
   ```

#### **Step 5: Test Everything**
1. **Test each recreated automation**
2. **Verify notifications work** with new buttons
3. **Test silence functionality**
4. **Confirm AI analysis** still works as expected

---

## 🎛️ Configuration Examples

### Example 1: Basic Front Door Monitoring
```yaml
# Simple person detection with interactive buttons
Frigate Camera: camera.front_door
Required Objects: ["person"]
Zones: ["front_porch"]
Severity: ["alert", "detection"]

# Interactive Buttons (New Event)
Button 1: "View Live" → VIEW_LIVE
Button 2: "Silence" → SILENCE  
Button 3: "Call Security" → CALL

# AI Analysis
Snapshot Prompt: "Describe who is at the front door and what they appear to be doing"
Clip Prompt: "Analyze the person's behavior. Are they delivering something, visiting, or suspicious?"
```

### Example 2: Advanced Driveway Security
```yaml
# Vehicle detection with person vacancy checking
Frigate Camera: camera.driveway
Required Objects: ["car", "person"]  # Requires BOTH objects
Zones: ["driveway"]

# Smart Filtering
Vacant Zone: zone.home
Persons Away: ["person.john", "person.jane"]  # Only alert when family is away
Custom Condition: "{{ now().hour >= 22 or now().hour <= 6 }}"  # Night only

# AI Analysis
Snapshot Prompt: "Vehicle and person detected in driveway while family is away. Describe the activity"
Clip Prompt: "Analyze this security event. Is this suspicious activity or legitimate?"
```

### Example 3: Package Delivery Detection
```yaml
# Smart package monitoring
Frigate Camera: camera.front_door
Required Objects: ["person"]
Zones: ["front_porch", "steps"]
Sub Labels: ["delivery", "package"]

# Notification Buttons
Button 1: "View Snapshot" → VIEW_SNAPSHOT
Button 2: "View Full Clip" → VIEW_CLIP
Button 3: "Silence 30min" → SILENCE

# AI Prompts
Snapshot Prompt: "Focus on package delivery activity. Describe what the person is doing with any packages"
Clip Prompt: "Analyze the complete delivery process. Was a package delivered or picked up?"
```

## 📱 Interactive Button Features

### Available Button Actions

| Action | Description | Use Case |
|--------|-------------|----------|
| `VIEW_LIVE` | Open live camera feed | Quick security check |
| `VIEW_SNAPSHOT` | View event snapshot | See what triggered alert |
| `VIEW_CLIP` | View full video clip | Review complete event |
| `SILENCE` | Pause notifications | Temporary quiet period |
| `CALL` | Dial phone number | Contact security/family |

### Smart Silence Feature
```yaml
# User interaction examples:
Tap "Silence" → 5 minutes (default)
Type "30" + Send → 30 minutes
Type "120" + Send → 2 hours

# System behavior:
- Automation temporarily disabled
- Visual confirmation shown
- Automatic re-enable after timer
- Affects only that specific automation
```

### Button Configuration
```yaml
# New Event Buttons
Button 1: "View Live" → VIEW_LIVE → Opens camera entity
Button 2: "Silence" → SILENCE → Enables text input for timer
Button 3: "Call" → CALL → Dials configured number

# End Event Buttons  
Button 1: "View Clip" → VIEW_CLIP → Opens video file
Button 2: "View Live" → VIEW_LIVE → Opens camera entity
Button 3: "Silence" → SILENCE → Enables text input for timer
```

## 🎯 Advanced Template Variables

Use these in custom messages and AI prompts:

### Event Information
```jinja2
{{input_objects}}      # Objects required: ["person", "car"]
{{objects}}            # Objects detected: ["person"]
{{camera_name}}        # Camera name: "Front Door"
{{zone_names}}         # Zones required: ["front_porch"]
{{before_zones}}       # Initial zones: ["driveway"]  
{{after_zones}}        # Final zones: ["front_porch"]
{{detections[0]}}      # Event ID: "1642095978.456789-abc123"
{{review_id}}          # Review ID for clips
```

### Custom Logic Examples
```jinja2
# Time-based conditions
{{ now().hour >= 22 or now().hour <= 6 }}  # Night only
{{ now().weekday() < 5 }}                   # Weekdays only

# Person presence
{{ is_state('person.john', 'home') }}       # John is home
{{ states('zone.home') | int > 0 }}         # Anyone home

# Weather integration  
{{ states('weather.home') == 'rainy' }}     # Weather conditions
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
- claude-3-5-sonnet-latest
- gemini-2.0-flash
```

### Token Optimization
```yaml
# Snapshot Analysis: 50-100 tokens (quick description)
# Clip Analysis: 100-200 tokens (detailed analysis)
# Balance: Speed vs Detail based on your needs
```

### Storage Management
```yaml
# Optional Downloader setup:
Root Directory: "/config/www/frigate"
Sub Directory: "{{now().strftime('%Y-%m-%d')}}"  # Daily folders
Cleanup: Implement periodic deletion of old files
```

## 🤝 Contributing

We welcome contributions! Here's how to help:

### Bug Reports
- Use GitHub Issues with detailed error logs
- Include Home Assistant and integration versions
- Provide configuration examples that reproduce the issue

### Feature Requests  
- Describe the use case and expected behavior
- Consider how it integrates with existing features
- Provide examples of how you'd configure it

### Pull Requests
- Fork the repository and create a feature branch
- Test thoroughly with multiple scenarios  
- Update documentation for any new features
- Follow existing code style and patterns

## 🙏 Acknowledgments

- **[Frigate](https://github.com/blakeblackshear/frigate)** - Outstanding NVR software
- **[LLM Vision](https://github.com/valentinfrlch/ha-llmvision)** - Excellent AI vision integration  
- **[Home Assistant Community](https://community.home-assistant.io/)** - Continuous support and feedback
- **Beta Testers** - Community members who helped refine v0.6 features

---

**Version**: v0.6  
**Author**: whag  
**Last Updated**: August 2025  
**Compatibility**: Home Assistant 2024.1+, Frigate 0.13+

## 📚 Quick Links

- [Blueprint File](Latest.yaml) - Ready to import
- [GitHub Issues](../../issues) - Bug reports and feature requests  
- [Home Assistant Community](https://community.home-assistant.io/) - General discussion
- [Frigate Documentation](https://docs.frigate.video/) - Camera setup guide
- [LLM Vision Setup](https://github.com/valentinfrlch/ha-llmvision) - AI provider configuration
