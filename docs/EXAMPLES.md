# Examples and Use Cases - Frigate LLM Notification

This document provides real-world configuration examples and use cases for the Frigate LLM Notification automation.

## 🏠 Home Security Use Cases

### 1. Front Door Visitor Detection

**Scenario**: Monitor front door for visitors, deliveries, and suspicious activity.

```yaml
# Configuration
Camera: camera.front_door
Zones: [front_porch, steps]
Objects: [person]
All Zones: false

# Snapshot Analysis
LLM Vision Snapshot: true
Prompt: |
  Analyze this front door security image. Focus on the person in the front porch area.
  Describe:
  1. Who appears to be at the door (delivery person, visitor, unknown individual)
  2. What they are carrying or doing
  3. Whether this appears to be normal visitor activity or requires attention
  4. Their apparent intent (delivering package, knocking, waiting, leaving)
  
  Rate the concern level as LOW (expected visitor/delivery), MEDIUM (unknown but normal behavior), 
  or HIGH (suspicious behavior) and explain why.

# Video Analysis  
Clip Prompt: |
  Track the person's complete interaction with the front door area.
  Describe the sequence of events: How they approached, what they did at the door,
  how long they stayed, and how they left. Note any packages delivered or picked up.
  Assess if this was a normal visitor interaction or if there are any concerns.

# Notification Settings
Click Action New Event: "{{image_local}}"
Click Action End Event: "{{video_local}}"
Cooldown: 3
```

**Expected Results**:
- "LOW concern: Delivery person in uniform placed package at front door and left"
- "MEDIUM concern: Unknown visitor knocked and waited 2 minutes before leaving"
- "HIGH concern: Individual approached door, looked around suspiciously, then left quickly without knocking"

### 2. Driveway Vehicle Monitoring

**Scenario**: Monitor driveway for family vehicles, deliveries, and unauthorized access.

```yaml
# Configuration
Camera: camera.driveway
Zones: [driveway, street_approach]
Objects: [car, truck, motorcycle, person]
All Zones: false

# Snapshot Analysis
Prompt: |
  Analyze vehicle activity in the driveway area. Focus on:
  1. Vehicle type, color, and any visible markings (license plate, company logos)
  2. Whether this appears to be a family vehicle, delivery vehicle, or unknown vehicle
  3. Any people getting in or out of the vehicle
  4. Suspicious behavior (slow driving, circling, parking without purpose)
  
  Classify as: FAMILY (known vehicle), DELIVERY (commercial vehicle/uniform), 
  VISITOR (normal guest), or CONCERN (suspicious activity).

# Video Analysis
Clip Prompt: |
  Track the complete vehicle interaction with the driveway.
  Describe: How the vehicle approached, where it parked, how long it stayed,
  what the occupants did, and how it left. Note any packages or items exchanged.
  Assess whether this was normal residential activity or warrants attention.

# Custom Actions - Turn on driveway lights for unknown vehicles
New Event Custom Actions:
  - choose:
      - conditions:
          - condition: template
            value_template: "{{ 'CONCERN' in response.response_text or 'unknown' in response.response_text.lower() }}"
        sequence:
          - action: light.turn_on
            target:
              entity_id: light.driveway_lights
            data:
              brightness: 255
              color_name: white
```

### 3. Backyard Pool Safety

**Scenario**: Monitor pool area for safety, especially when children might be present.

```yaml
# Configuration
Camera: camera.pool_area
Zones: [pool_deck, pool_water, fence_gate]
Objects: [person]
All Zones: false

# Snapshot Analysis
Prompt: |
  POOL SAFETY ANALYSIS: Examine this pool area image carefully.
  Focus on:
  1. Any people near or in the pool area - estimate age (adult, teenager, child)
  2. Swimming activity or risk of falling in water
  3. Gate position (open/closed) and any unsupervised access
  4. Safety equipment visible (life rings, pool covers, barriers)
  
  PRIORITY: Any child near water requires immediate attention.
  Rate as: SAFE (supervised activity), MONITOR (unsupervised adult), 
  or URGENT (child near water, safety concern).

# High priority for pool safety
Tokens: 150
Temperature: 0.2  # More accurate, less creative

# Immediate notification for urgent situations
Cooldown: 1

# Custom Actions - Alert all devices for urgent pool situations
New Event Custom Actions:
  - choose:
      - conditions:
          - condition: template
            value_template: "{{ 'URGENT' in response.response_text }}"
        sequence:
          - action: notify.all_devices
            data:
              title: "🚨 POOL SAFETY ALERT"
              message: "{{ response.response_text }}"
              data:
                notification_icon: "mdi:pool"
                importance: high
                interruption-level: critical
```

## 🚛 Delivery and Package Management

### 4. Package Delivery Detection

**Scenario**: Track all package deliveries with detailed logging and family notifications.

```yaml
# Configuration
Camera: camera.front_porch
Zones: [front_porch, steps, driveway]
Objects: [person]
All Zones: false

# Snapshot Analysis
Prompt: |
  PACKAGE DELIVERY ANALYSIS: Focus specifically on delivery activity.
  Look for:
  1. Delivery company uniforms or vehicles (UPS, FedEx, Amazon, USPS)
  2. Packages being carried, placed, or picked up
  3. Delivery confirmation actions (photos, door knocking, doorbell)
  4. Package size and placement location
  
  If this IS a delivery: Describe the company, package details, and placement.
  If this is NOT a delivery: Briefly describe what the person is actually doing.
  
  Format: "DELIVERY: [Company] delivered [size] package to [location]" 
  OR "NOT DELIVERY: [brief description]"

# Video Analysis
Clip Prompt: |
  Track the complete delivery process from arrival to departure.
  Describe: Delivery company, how they arrived, delivery process 
  (knocked/rang bell, placed package, took photo), where package was left,
  and any interaction with residents. Note professionalism and proper procedures.

# Custom Actions - Log deliveries and notify family
New Event Custom Actions:
  - choose:
      - conditions:
          - condition: template
            value_template: "{{ 'DELIVERY:' in response.response_text }}"
        sequence:
          # Log delivery to calendar
          - action: calendar.create_event
            target:
              entity_id: calendar.deliveries
            data:
              summary: "Package Delivered - {{ response.response_text.split(':')[1].split('delivered')[0].strip() }}"
              start_date_time: "{{ now().isoformat() }}"
              end_date_time: "{{ (now() + timedelta(minutes=30)).isoformat() }}"
          
          # Notify family group
          - action: notify.family_group
            data:
              title: "📦 Package Delivered"
              message: "{{ response.response_text }}"
              data:
                image: "{{ image_local }}"
                actions:
                  - action: "RETRIEVED"
                    title: "Mark Retrieved"
                  - action: "VIEW_CAMERA"
                    title: "View Camera"

End Event Custom Actions:
  - action: input_boolean.turn_on
    target:
      entity_id: input_boolean.package_waiting
```

### 5. Mail and Small Package Detection

**Scenario**: Monitor mailbox area for mail delivery and small packages.

```yaml
# Configuration
Camera: camera.mailbox
Zones: [mailbox_area, street]
Objects: [person, truck]
All Zones: false

# Snapshot Analysis
Prompt: |
  MAIL DELIVERY MONITORING: Analyze activity around the mailbox area.
  Focus on:
  1. USPS mail carrier uniform and vehicle
  2. Mail being placed in or retrieved from mailbox
  3. Small packages left near mailbox
  4. Non-mail related activity in the area
  
  Classify as:
  - MAIL DELIVERY: Official postal service activity
  - PACKAGE DELIVERY: Packages left by mailbox
  - MAIL RETRIEVAL: Resident collecting mail
  - UNRELATED: Other activity in area
  
  For deliveries, note if items are secure or might need attention.

# Reduce processing for faster mail notifications
Tokens: 75
Max Frames: 2
Delay Before Image: 1
```

## 👨‍👩‍👧‍👦 Family and Visitor Management

### 6. Family Member Recognition

**Scenario**: Differentiate between family members and unknown visitors.

```yaml
# Configuration
Camera: camera.front_door
Zones: [front_porch, driveway]
Objects: [person]

# Enable memory for family recognition
Use Memory: true
Use Remember: true

# Snapshot Analysis
Prompt: |
  FAMILY RECOGNITION: Analyze this person approaching the house.
  
  Using your memory of family members and frequent visitors:
  1. Do you recognize this person as a family member or known visitor?
  2. If recognized, who is it and how are they approaching (walking, with keys, etc.)?
  3. If unrecognized, describe their appearance and behavior
  4. Note any vehicles associated with this person
  
  Classify as: FAMILY (known family member), KNOWN VISITOR (recognized guest), 
  or UNKNOWN VISITOR (unrecognized person).
  
  For family members, you can be brief. For unknown visitors, provide more detail.

# Different actions for family vs strangers
New Event Custom Actions:
  - choose:
      # Family member - low key notification
      - conditions:
          - condition: template
            value_template: "{{ 'FAMILY' in response.response_text }}"
        sequence:
          - action: notify.parent_devices
            data:
              title: "🏠 {{ response.response_text.split(':')[1].split('arrived')[0].strip() }} Home"
              message: "{{ response.response_text }}"
              data:
                notification_icon: "mdi:home-account"
                importance: low
      
      # Unknown visitor - alert all devices
      - conditions:
          - condition: template
            value_template: "{{ 'UNKNOWN' in response.response_text }}"
        sequence:
          - action: notify.all_devices
            data:
              title: "🚨 Unknown Visitor"
              message: "{{ response.response_text }}"
              data:
                image: "{{ image_local }}"
                importance: high
                actions:
                  - action: "VIEW_LIVE"
                    title: "View Live"
                  - action: "CALL_POLICE"
                    title: "Emergency"
```

### 7. Children Activity Monitoring

**Scenario**: Monitor children's outdoor activities and ensure safety.

```yaml
# Configuration
Camera: camera.backyard
Zones: [play_area, garden, fence_line]
Objects: [person]

# Snapshot Analysis
Prompt: |
  CHILD SAFETY MONITORING: Analyze activity in the backyard play area.
  
  Focus on:
  1. Number of children present and estimated ages
  2. Type of play activity (safe play, climbing, near hazards)
  3. Adult supervision present or absent
  4. Any safety concerns (dangerous activities, accessing restricted areas)
  5. Weather appropriateness of activity
  
  Rate safety level:
  - SAFE: Normal supervised play
  - MONITOR: Unsupervised but safe activity
  - CONCERN: Potentially unsafe situation requiring attention

# Shorter cooldown for child safety
Cooldown: 2

# Custom Actions - Different responses based on safety level
New Event Custom Actions:
  - choose:
      # Safe activity - just log
      - conditions:
          - condition: template
            value_template: "{{ 'SAFE:' in response.response_text }}"
        sequence:
          - action: logbook.log
            data:
              name: "Child Activity Log"
              message: "{{ response.response_text }}"
      
      # Concern - immediate alert
      - conditions:
          - condition: template
            value_template: "{{ 'CONCERN:' in response.response_text }}"
        sequence:
          - action: notify.parent_devices
            data:
              title: "⚠️ Child Safety Alert"
              message: "{{ response.response_text }}"
              data:
                image: "{{ image_local }}"
                importance: high
                interruption-level: active
```

## 🏢 Business and Commercial Use Cases

### 8. Store Customer Behavior Analysis

**Scenario**: Monitor customer activity for business insights and security.

```yaml
# Configuration
Camera: camera.store_entrance
Zones: [entrance, display_area, checkout]
Objects: [person]

# Snapshot Analysis
Prompt: |
  CUSTOMER BEHAVIOR ANALYSIS: Analyze customer activity in the store.
  
  Observe:
  1. Number of customers and general demographics
  2. Customer behavior (browsing, focused shopping, loitering)
  3. Areas of interest (which displays they're viewing)
  4. Traffic flow patterns
  5. Any security concerns or suspicious behavior
  
  Provide insights on: Customer engagement level, popular areas, 
  and any operational notes for store management.

# Video Analysis
Clip Prompt: |
  Track customer journey through the store areas.
  Analyze: Entry behavior, time spent in different zones, 
  items they examined, checkout process, and exit behavior.
  Note any operational insights or customer service opportunities.

# Custom Actions - Business intelligence logging
End Event Custom Actions:
  - action: rest_command.customer_analytics
    data:
      timestamp: "{{ now().isoformat() }}"
      camera: "{{ camera_name }}"
      analysis: "{{ response.response_text }}"
      duration: "{{ (now() - strptime(event.after.start_time, '%Y-%m-%dT%H:%M:%S%z')).total_seconds() }}"
```

### 9. Warehouse Loading Dock Monitoring

**Scenario**: Monitor loading dock for deliveries, safety, and operational efficiency.

```yaml
# Configuration
Camera: camera.loading_dock
Zones: [dock_area, truck_bay, safety_zone]
Objects: [person, truck, forklift]

# Snapshot Analysis
Prompt: |
  LOADING DOCK OPERATIONS: Analyze activity at the loading dock.
  
  Safety and Operations Focus:
  1. PPE compliance (hard hats, safety vests, proper footwear)
  2. Vehicle positioning and safety clearances
  3. Loading/unloading procedures being followed
  4. Personnel in safety zones or restricted areas
  5. Equipment operation (forklifts, dock plates)
  
  Rate as: COMPLIANT (safe operations), MINOR ISSUE (attention needed), 
  or SAFETY VIOLATION (immediate intervention required).

# High accuracy for safety
Temperature: 0.1
Tokens: 200

# Custom Actions - Safety violation alerts
New Event Custom Actions:
  - choose:
      - conditions:
          - condition: template
            value_template: "{{ 'SAFETY VIOLATION' in response.response_text }}"
        sequence:
          - action: notify.safety_team
            data:
              title: "🚨 DOCK SAFETY VIOLATION"
              message: "{{ response.response_text }}"
              data:
                image: "{{ image_local }}"
                importance: critical
          
          - action: siren.turn_on
            target:
              entity_id: siren.dock_warning
```

## 🚗 Parking and Vehicle Management

### 10. Parking Lot Monitoring

**Scenario**: Monitor parking areas for space availability and unauthorized parking.

```yaml
# Configuration
Camera: camera.parking_lot
Zones: [visitor_parking, reserved_parking, fire_lane]
Objects: [car, truck, motorcycle]

# Snapshot Analysis
Prompt: |
  PARKING MANAGEMENT: Analyze vehicle parking in the lot.
  
  Check for:
  1. Vehicles in designated visitor vs reserved spaces
  2. Proper parking within space boundaries
  3. Fire lane or emergency access violations
  4. Handicap space compliance
  5. Available parking spaces
  6. Vehicle types and estimated parking duration
  
  Note any violations or management issues requiring attention.

# Video Analysis
Clip Prompt: |
  Track vehicle parking behavior from arrival to final position.
  Note: How they searched for parking, adherence to traffic flow,
  parking accuracy, and any violations. Assess if this was 
  considerate parking or if intervention is needed.

# Custom Actions - Parking violation notifications
New Event Custom Actions:
  - choose:
      - conditions:
          - condition: template
            value_template: "{{ 'fire lane' in response.response_text.lower() or 'violation' in response.response_text.lower() }}"
        sequence:
          - action: notify.security_team
            data:
              title: "🚫 Parking Violation Detected"
              message: "{{ response.response_text }}"
              data:
                image: "{{ image_local }}"
                actions:
                  - action: "DISPATCH_SECURITY"
                    title: "Send Security"
                  - action: "ISSUE_TICKET"
                    title: "Issue Citation"
```

## 🎯 Advanced Integration Examples

### 11. Smart Home Integration

**Scenario**: Integrate security events with complete smart home automation.

```yaml
# Configuration includes multiple cameras and comprehensive automation

# Custom Actions - Comprehensive smart home response
New Event Custom Actions:
  - parallel:
      # Security system integration
      - action: alarm_control_panel.alarm_arm_away
        target:
          entity_id: alarm_control_panel.house
        data:
          code: "{{ states('input_text.alarm_code') }}"
      
      # Lighting automation
      - action: scene.turn_on
        target:
          entity_id: scene.security_alert
      
      # Climate adjustment
      - action: climate.set_preset_mode
        target:
          entity_id: climate.house
        data:
          preset_mode: "away"
      
      # Voice announcements
      - action: tts.google_translate_say
        target:
          entity_id: group.all_speakers
        data:
          message: "Security alert: {{ response.response_text }}"
      
      # External monitoring service
      - action: rest_command.security_monitoring
        data:
          event_type: "{{ type }}"
          camera: "{{ camera_name }}"
          timestamp: "{{ now().isoformat() }}"
          ai_analysis: "{{ response.response_text }}"
          severity: >-
            {% if 'HIGH' in response.response_text or 'URGENT' in response.response_text %}
              critical
            {% elif 'MEDIUM' in response.response_text or 'CONCERN' in response.response_text %}
              warning
            {% else %}
              info
            {% endif %}
```

### 12. Multi-Camera Coordination

**Scenario**: Coordinate multiple cameras for comprehensive coverage.

```yaml
# Use separate automations for each camera but coordinate responses

# Custom Actions - Cross-camera coordination
New Event Custom Actions:
  # Check if other cameras have recent events
  - condition: template
    value_template: >-
      {{ (now() - states.automation.front_door_security.attributes.last_triggered).total_seconds() < 300 }}
  
  # Multi-camera event correlation
  - action: script.security_event_correlation
    data:
      primary_camera: "{{ camera_name }}"
      event_id: "{{ id }}"
      ai_description: "{{ response.response_text }}"
      
  # Coordinate PTZ cameras to focus on activity
  - action: camera.turn_on
    target:
      entity_id: camera.ptz_overview
  - action: onvif.ptz_preset
    target:
      entity_id: camera.ptz_overview
    data:
      preset: "{{ camera_name }}_focus"
```

---

## 💡 Tips for Creating Custom Use Cases

### 1. Start Simple
- Begin with basic person detection
- Add complexity gradually
- Test each component before combining

### 2. Optimize Prompts
- Be specific about what you want to detect
- Include context about normal vs concerning behavior
- Use consistent terminology for classification

### 3. Plan Custom Actions
- Consider what should happen after detection
- Think about notification urgency levels
- Plan for integration with existing systems

### 4. Performance Considerations
- Balance detail with response time
- Use appropriate models for your use case
- Consider token limits and costs

### 5. Privacy and Ethics
- Ensure compliance with local privacy laws
- Consider notification to people being monitored
- Implement appropriate data retention policies

These examples provide a foundation for creating your own custom security and monitoring solutions. Adapt the configurations to match your specific environment, needs, and integration requirements.
