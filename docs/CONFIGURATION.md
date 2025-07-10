# Configuration Guide - Frigate LLM Notification

This guide provides detailed configuration options and examples for the Frigate LLM Notification automation.

## 📋 Configuration Overview

The blueprint is organized into logical sections for easy configuration:

1. **Frigate Options** - Camera and detection settings
2. **LLM Vision Provider** - AI model configuration
3. **Snapshot Analysis** - Image processing settings
4. **Review Clip Analysis** - Video processing settings
5. **Downloader Options** - File storage settings
6. **Notification Options** - Mobile notification settings
7. **Custom Actions** - Additional automation triggers

## 🎥 Frigate Options

### Camera Selection
```yaml
# Select the Frigate camera to monitor
Frigate Camera: camera.front_door
```

**Important**: Only Frigate cameras will appear in the dropdown. Ensure your camera is properly configured in Frigate and the integration is working.

### Zone Configuration
```yaml
# Select specific zones for monitoring
Frigate Zones: 
  - front_door

# Or use custom zone names
Custom Zones:
  - "custom_zone_1"
  - "backyard_pool"
```

**Zone Logic Options**:
- **Any Zone (default)**: Alert if motion in ANY selected zone
- **All Zones**: Alert only if motion in ALL selected zones

### Object Detection Filtering
```yaml
# Filter by specific objects
Required Objects:
  - person
  - car
  - bicycle
  - dog

# Leave empty to detect all objects
Required Objects: []
```

**Available Objects**: person, bicycle, car, motorcycle, airplane, bus, train, truck, boat, traffic light, fire hydrant, street sign, stop sign, parking meter, bench, bird, cat, dog, horse, sheep, cow, elephant, bear, zebra, giraffe, hat, backpack, umbrella, shoe, eye glasses, handbag, tie, suitcase, frisbee, skis, snowboard, sports ball, kite, baseball bat, baseball glove, skateboard, surfboard, tennis racket, bottle, plate, wine glass, cup, fork, knife, spoon, bowl, banana, apple, sandwich, orange, broccoli

### Sub-Label Filtering
```yaml
# Filter by Frigate sub-labels (if configured)
Sub Labels Required:
  - "delivery_person"
  - "family_member"
  - "neighbor"
```

## 🤖 LLM Vision Provider Configuration

### Provider Selection
Choose your AI provider from the configured integrations:
- **OpenAI** (GPT models)
- **Anthropic** (Claude models)
- **Google** (Gemini models)
- **Groq** (Llama models)
- **Local providers** (Ollama, etc.)

### Model Selection
```yaml
# Recommended models by use case:

# Fast real-time alerts:
Model: gpt-4o-mini
Model: gemini-1.5-flash
Model: claude-3-5-haiku-latest

# High-quality analysis:
Model: gpt-4o
Model: gemini-1.5-pro
Model: claude-3-5-sonnet-latest

# Cost-effective options:
Model: gpt-4o-mini
Model: gemini-2.0-flash-lite
```

## 📸 Snapshot Analysis Configuration

### AI Analysis Toggle
```yaml
# Enable AI analysis for snapshots
LLM Vision Snapshot: true

# Use custom message instead
LLM Vision Snapshot: false
```

### Custom Notification Content
```yaml
# Notification title (when AI disabled)
No AI Notification Title: "{{camera_name}} - {{sub_labels|join(',')|replace('_', ' ')}} Alert"

# Notification message (when AI disabled)
No AI Notification Message: "A {{objects|join(' and a ')|replace('_', ' ')}} was detected in the {{after_zones|join(' and the ')|replace('_', ' ')}}"
```

### AI Prompt Configuration
```yaml
# Basic prompt example
Prompt: |
  Summarise the image in a 1 sentence response,
  concentrate on the {{input_objects|join(',')|replace('_', ' ')}} if seen within
  the {{zone_names|join(' and ,')|replace('_',' ')}}.
  Any person approaching the camera should take priority over all other events.
  If you see a known person from memory, mention this so the response.
  Do not mention the prompt or describe the environment in your response.
  The response is an intial alert for a mobile phone security notification.

# Advanced prompt examples
Security Focused Prompt: |
  Analyze this security camera image. Focus on {{input_objects|join(', ')}} 
  in the {{zone_names|join(' and ')}} area. 
  Describe the activity level (low/medium/high concern), 
  what the person/object is doing, and if this appears to be 
  normal activity or requires attention.

Delivery Detection Prompt: |
  Focus on package delivery activity in the {{zone_names|join(' and ')}} area.
  Look for delivery personnel, packages being dropped off or picked up,
  and vehicles that might be delivery trucks or cars.
  Describe what delivery activity you observe or if this is unrelated activity.

Visitor Analysis Prompt: |
  Analyze visitor activity at the {{zone_names|join(' and ')}} area.
  Describe who appears to be visiting (delivery person, family member, stranger),
  what they're doing (approaching door, leaving package, waiting),
  and their apparent intent.
```

### Performance Settings
```yaml
# Image processing
Pixels: 1080          # Width in pixels (512-1920)
Tokens: 100           # Maximum response tokens (1-300)
Response Creativity: 0.5  # Temperature (0.0-1.0)

# Processing delays
Delay Before Image: 2     # Seconds to wait before analysis
```

### AI Features
```yaml
# Generate AI titles for notifications
Generate Title: true

# Use LLM Vision memory for context
Use Memory: true

# Store events in LLM Vision timeline
Use Remember: true

# Save images to LLM Vision
Expose Image: true
```

## 🎬 Review Clip Analysis Configuration

### Clip Analysis Prompt
```yaml
# Video analysis prompt
Prompt: |
  Analyse the video clip which should contain the following objects
  {{input_objects|join(',')|replace('_', ' ')}}
  The areas of interest are the {{zone_names|join(' and ,')|replace('_',' ')}}
  Focus on the objects referenced which are entering the areas of interest 
  and describe what they are doing.
  Consider what the {{input_objects|join(',')|replace('_',' ')}} is doing, 
  why, and what it might do next.
  Do not mention the prompt or describe the environment in your response.
  Your response will be used for a mobile phone notification message 
  with the clip attached.

# Security-focused video prompt
Security Video Prompt: |
  Analyze this security video clip for the {{zone_names|join(' and ')}} area.
  Track the movement and behavior of {{input_objects|join(', ')}} throughout the clip.
  Describe: 1) What happened during this event, 2) The person/object's behavior,
  3) Whether this appears normal or concerning, 4) Any notable details.
  Provide a concise security summary suitable for a mobile alert.
```

### Video Processing Settings
```yaml
# Video analysis settings
Pixels: 1080              # Video width (512-1920)
Tokens: 100               # Maximum response tokens
Max Frames: 3             # Frames to analyze (1-10)

# Processing delays
Delay Before Clip: 2      # Seconds to wait before analysis
```

## 💾 Downloader Configuration

### Directory Setup
```yaml
# Root directory for downloads
Downloader Directory: downloads

# Sub-directory organization
Downloader Sub Directory: frigate_events
# Results in: downloads/frigate_events/

# Date-based organization example
Downloader Sub Directory: "{{ now().strftime('%Y/%m/%d') }}"
# Results in: downloads/2025/01/15/

# Camera-based organization
Downloader Sub Directory: "{{ camera_name }}"
# Results in: downloads/front_door/
```

### Download Timeouts
```yaml
# File download timeouts
Image Download Max Wait Time: 5   # Seconds (1-120)
Clip Download Max Wait Time: 30   # Seconds (1-120)
```

**File Naming Convention**:
- **Snapshots**: `{event_id}_snapshot.jpg`
- **Clips**: `{event_id}_clip.mp4`

## 📱 Notification Configuration

### Device Selection
```yaml
# Select mobile devices for notifications
Notify Devices:
  - device_tracker.johns_phone
  - device_tracker.janes_iphone
```

### Click Actions
```yaml
# Action when tapping new event notification
Click Action New Event: /lovelace
# Options:
# - /lovelace (dashboard)
# - "{{image_local}}" (downloaded snapshot)
# - "{{image}}" (Frigate snapshot)
# - Custom URL

# Action when tapping end event notification  
Click Action End Event: "{{video_local}}"
# Options:
# - /lovelace (dashboard)
# - "{{image_local}}" (downloaded snapshot)
# - "{{video_local}}" (downloaded clip)
# - "{{video}}" (Frigate clip)
# - "{{gif}}" (Frigate GIF preview)
```

### Notification Timing
```yaml
# Cooldown between notifications
Cooldown: 2  # Minutes (0-60)
```

## 🔧 Custom Actions

### New Event Actions
```yaml
# Example: Turn on lights when person detected
New Event Custom Actions:
  - action: light.turn_on
    target:
      entity_id: light.front_porch
    data:
      brightness: 255
      color_name: white
      
  - action: script.security_alert
    data:
      camera: "{{ camera_name }}"
      event_id: "{{ id }}"
```

### End Event Actions
```yaml
# Example: Save important clips
End Event Custom Actions:
  - action: shell_command.backup_clip
    data:
      file_path: "{{ video_local }}"
      event_id: "{{ id }}"
      
  - action: persistent_notification.create
    data:
      title: "Security Event Complete"
      message: "Event {{ id }} at {{ camera_name }} has ended"
```

## 🌟 Advanced Configuration Examples

### Multi-Zone Package Detection
```yaml
Camera: camera.front_door
Zones: [front_porch, steps, driveway]
Objects: [person]
All Zones: false  # Alert if person in ANY zone

Snapshot Prompt: |
  Focus on package delivery activity. Look for:
  1. Delivery personnel with packages or uniforms
  2. Packages being placed or picked up
  3. Delivery vehicles in the driveway
  Describe the delivery activity or state if this appears unrelated to deliveries.

Clip Prompt: |
  Analyze the complete delivery event. Track the person's movement
  through the zones and describe: What was delivered? Where was it placed?
  Did the person follow proper delivery procedures? Any concerns?
```

### Vehicle Security Monitoring
```yaml
Camera: camera.driveway
Zones: [driveway, street]
Objects: [car, truck, motorcycle, person]
All Zones: false

Snapshot Prompt: |
  Analyze vehicle activity in the driveway area. Focus on:
  1. Known vs unknown vehicles
  2. Suspicious behavior (lingering, slow driving)
  3. People getting in/out of vehicles
  Rate the security concern level (low/medium/high) and explain why.

Clip Prompt: |
  Track vehicle movement and occupant behavior throughout this event.
  Describe: Vehicle type and color, how long it stayed, what occupants did,
  whether this appears to be normal visitor activity or potential security concern.
```

### Smart Home Integration
```yaml
New Event Custom Actions:
  # Announce through smart speakers
  - action: tts.google_translate_say
    target:
      entity_id: media_player.living_room_speaker
    data:
      message: "Security alert: {{ ai_description }}"
      
  # Send to security system
  - action: alarm_control_panel.alarm_trigger
    target:
      entity_id: alarm_control_panel.house
    data:
      code: "1234"
      
  # Log to external system
  - action: rest_command.security_log
    data:
      event_id: "{{ id }}"
      camera: "{{ camera_name }}"
      timestamp: "{{ now().isoformat() }}"
      description: "{{ ai_description }}"
```

## ⚠️ Important Notes

### Performance Considerations
- **Model Speed**: Faster models provide quicker notifications but may be less detailed
- **Token Limits**: Higher token counts provide more detail but slower responses
- **Frame Analysis**: More frames provide better video understanding but slower processing

### Storage Management
- **Automatic Cleanup**: Consider implementing periodic cleanup of downloaded files
- **Disk Space**: Monitor available storage to prevent system issues
- **File Organization**: Use organized directory structures for easier management

### Privacy and Security
- **AI Provider**: Ensure your AI provider's privacy policy meets your requirements
- **Local Storage**: Consider local AI models for sensitive environments
- **Network Security**: Secure your Frigate and Home Assistant instances appropriately

---

**Next Steps**: After configuration, test thoroughly with different scenarios and tune settings based on your specific needs and environment.
