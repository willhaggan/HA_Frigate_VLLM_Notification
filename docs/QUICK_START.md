# Quick Start Guide - Frigate LLM Notification

This guide will help you get the Frigate LLM Notification automation up and running quickly.

## ⚡ Prerequisites Checklist

Before starting, ensure you have:

- [ ] **Home Assistant** 2023.1+ installed and running
- [ ] **MQTT Broker** configured (Mosquitto recommended)
- [ ] **Frigate NVR** installed with camera feeds working
- [ ] **Mobile device** with Home Assistant Companion App
- [ ] **AI Provider account** (OpenAI, Google, Anthropic, etc.)

## 🚀 15-Minute Setup

### Step 1: Install Required Integrations (5 minutes)

#### 1.1 Frigate Integration
```bash
# Install via HACS (recommended)
HACS → Integrations → Search "Frigate" → Install
```
Or manually add to `configuration.yaml`:
```yaml
frigate:
  host: your_frigate_server_ip
  port: 5000
```

#### 1.2 LLM Vision Integration
```bash
# Install via HACS
HACS → Integrations → Search "LLM Vision" → Install
```

#### 1.3 Downloader Integration
The Downloader integration is built into Home Assistant. Enable it in:
```yaml
# configuration.yaml
downloader:
  download_dir: downloads
```

### Step 2: Configure AI Provider (3 minutes)

#### For OpenAI (GPT):
1. Go to **Settings → Integrations → Add Integration**
2. Search for **"LLM Vision"**
3. Select **OpenAI** as provider
4. Enter your **API key**
5. Choose **gpt-4o-mini** for fast responses

#### For Google (Gemini):
1. Follow the same steps but select **Google**
2. Enter your **API key**
3. Choose **gemini-1.5-flash** for optimal performance

### Step 3: Import Blueprint (2 minutes)

1. **Copy the blueprint URL**:
   ```
   https://github.com/willhaggan/HA_Frigate_VLLM_Notification/blob/main/Latest.yaml
   ```

2. **Import in Home Assistant**:
   - Go to **Settings → Automations & Scenes → Blueprints**
   - Click **"Import Blueprint"**
   - Paste the URL and click **"Preview Blueprint"**
   - Click **"Import Blueprint"**

### Step 4: Create Automation (5 minutes)

1. **Create new automation**:
   - Go to **Settings → Automations & Scenes → Automations**
   - Click **"Create Automation"**
   - Select **"Use Blueprint"**
   - Choose **"Frigate LLM Notification Latest"**

2. **Basic Configuration**:
   ```yaml
   # Frigate Options
   Camera: camera.your_camera_name
   Zones: [front_door, driveway]  # Select relevant zones
   Objects: [person, car]         # Objects to detect
   
   # LLM Vision
   Provider: Your configured provider
   Model: gpt-4o-mini (or gemini-1.5-flash)
   
   # Notifications
   Devices: [your_phone]          # Select your mobile device
   ```

3. **Save the automation** with a descriptive name

## 🎯 Basic Configuration Examples

### Front Door Monitoring
```yaml
Camera: camera.front_door
Zones: [front_porch]
Objects: [person]
Prompt: "Describe who is at the front door and what they appear to be doing."
```

### Driveway Vehicle Detection
```yaml
Camera: camera.driveway
Zones: [driveway]
Objects: [car, truck, motorcycle]
Prompt: "Describe the vehicle activity in the driveway."
```

### Package Delivery Detection
```yaml
Camera: camera.porch
Zones: [front_porch, steps]
Objects: [person]
Prompt: "Focus on package delivery. Is someone delivering or picking up packages?"
```

## ✅ Testing Your Setup

### Test 1: Manual Trigger
1. Walk in front of your camera in the configured zone
2. Check for notification on your mobile device
3. Verify the AI description makes sense

### Test 2: Different Objects
1. Have a car enter the driveway (if configured)
2. Check notification content and accuracy
3. Verify video clip analysis works

### Test 3: File Downloads
1. Check your Home Assistant downloads folder
2. Verify snapshot and clip files are saved
3. Confirm file names match the event IDs

## 🔧 Quick Troubleshooting

### No Notifications Received
```yaml
# Check these settings:
1. Mobile app notifications enabled
2. Device selected correctly in automation
3. Camera and zones configured properly
4. MQTT broker working (check Frigate events)
```

### AI Analysis Not Working
```yaml
# Verify:
1. LLM Vision integration configured
2. API key valid and has credits
3. Model selection correct
4. Prompt not too complex
```

### Files Not Downloading
```yaml
# Check:
1. Downloader integration enabled
2. Download directory exists and writable
3. Network access to Frigate server
4. Home Assistant IP address correct in config
```

## 📱 Mobile App Setup

### Android Setup
1. Install **Home Assistant Companion** from Play Store
2. Enable **notification permissions**
3. Set notification **importance to High**
4. Allow **background activity**

### iOS Setup
1. Install **Home Assistant Companion** from App Store
2. Enable **push notifications**
3. Set **notification delivery to Immediate**
4. Allow **background app refresh**

## 🎨 Customization Tips

### Improve AI Descriptions
```yaml
# Better prompts:
"Analyze this security event. Focus on [specific area] and describe any [specific activity] you observe. Mention if this appears normal or concerning."
```

### Optimize Performance
```yaml
# Fast models for real-time alerts:
- gpt-4o-mini
- gemini-1.5-flash
- claude-3-5-haiku-latest

# Limit tokens for speed:
Tokens: 50-100 for quick descriptions
Tokens: 100-200 for detailed analysis
```

### Reduce False Positives
```yaml
# Fine-tune zones:
- Create smaller, specific zones
- Exclude areas with frequent motion (trees, flags)
- Use object filtering (person only for doors)
```

## 📞 Getting Help

### Community Resources
- **[Home Assistant Community](https://community.home-assistant.io/)** - General HA help
- **[Frigate Discord](https://discord.gg/frigate)** - Frigate-specific support
- **[LLM Vision GitHub](https://github.com/valentinfrlch/ha-llmvision)** - AI integration issues

### Common Solutions
1. **Check logs**: Settings → System → Logs
2. **Test components individually**: Frigate → LLM Vision → Notifications
3. **Verify network connectivity**: HA to Frigate to AI provider
4. **Review configuration**: Compare with working examples

---

**⭐ Pro Tip**: Start with a simple configuration (one camera, one zone, person detection only) and gradually add complexity as you become comfortable with the system.

**Next Steps**: Once basic setup is working, explore advanced features like custom actions, memory integration, and multi-camera setups.
