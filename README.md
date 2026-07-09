# Frigate LLM Notification v0.8 

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2024.1%2B-blue.svg)](https://www.home-assistant.io/)
[![Frigate](https://img.shields.io/badge/Frigate-0.13%2B-green.svg)](https://github.com/blakeblackshear/frigate)
[![LLM Vision](https://img.shields.io/badge/LLM%20Vision-Integration-orange.svg)](https://github.com/valentinfrlch/ha-llmvision)

A Home Assistant blueprint that combines **Frigate NVR**, **LLM Vision AI**, and **actionable mobile notifications** for smart, context-aware security alerts. Fully **iOS and Android aware**.

## Features

- **iOS & Android aware** – HLS (`.m3u8`) clips on iOS, MP4 on Android, chosen automatically per device.
- **Two-stage AI analysis** – fast snapshot summary on the *new* event, deeper video analysis on the *end* event.
- **Interactive notifications** – 3 configurable buttons per event (View Live, Silence, Call, etc.), smart silence with a custom timer, and click-through to snapshot/clip/live view.
- **Smart filtering** – required objects, zones, sub-labels, severity, zone-vacancy (person-away) checks, plus a custom condition slot.
- **Flexible AI provider** – works with any LLM Vision provider/model (OpenAI, Anthropic, Google, Groq, local, etc.); memory & timeline integration optional.
- **Optional local caching** – use the Downloader integration to store snapshots/clips locally before analysis instead of streaming the URL directly.
- **Parallel-safe** – multiple cameras/events can run through the same automation instance without blocking each other.

## Prerequisites

- **[Frigate integration](https://github.com/blakeblackshear/frigate-hass-integration)** with MQTT set up and at least one camera/zone configured.
- **[LLM Vision integration](https://github.com/valentinfrlch/ha-llmvision)** with a provider configured (API key, model access).
- **[Home Assistant Companion App](https://companion.home-assistant.io/)** on Android/iOS with notifications enabled.
- **[Downloader integration](https://www.home-assistant.io/integrations/downloader/)** – optional, only needed if you enable local file caching.

## Installation

1. Import the blueprint: **Settings → Automations & Scenes → Blueprints → Import Blueprint**, then point it at [blueprints/automation/frigate_llm_notification_v0_8.yaml](blueprints/automation/frigate_llm_notification_v0_8.yaml) in this repo (or its raw URL if hosted on GitHub).
2. Create a new automation from the blueprint and fill in the minimum required inputs:
   - Frigate camera + required objects (e.g. `person`)
   - LLM Vision provider/model
   - Notify device(s), and which of them are iOS (for correct clip format)
3. Save, trigger a test detection, and confirm the notification arrives with working buttons.

## Example Configuration

```yaml
Frigate Camera: camera.front_door
Required Objects: [person]
Zones: [front_porch]
Severity: [alert, detection]

Notify Devices: [mobile_app_iphone, mobile_app_android]
iOS Devices: [mobile_app_iphone]        # needed for correct video format

Button 1: View Live   → VIEW_LIVE
Button 2: Silence     → SILENCE
Button 3: Call        → CALL

Snapshot Prompt: "Describe who is at the front door and what they appear to be doing."
Clip Prompt: "Analyze the person's behavior — are they delivering, visiting, or suspicious?"
```

## Notification Buttons

| Action | What it does |
|---|---|
| `VIEW_LIVE` | Opens the live camera feed |
| `VIEW_SNAPSHOT` | Opens the event snapshot |
| `VIEW_CLIP` | Opens the full clip (iOS/Android aware) |
| `SILENCE` | Pauses this automation; reply with a number to set custom minutes (default 5) |
| `CALL` | Dials the configured phone number |

## Useful Template Variables

```jinja2
{{ camera_name }}     # "Front Door"
{{ objects }}         # Objects detected, e.g. ["person"]
{{ zone_names }}      # Zones required, e.g. ["front_porch"]
{{ video }}           # MP4 clip URL (Android)
{{ video_ios }}       # HLS clip URL (iOS)
{{ id }}              # Frigate event ID
```

## Choosing a Model

The blueprint's model dropdowns list the current models supported by the LLM Vision providers used when this repo was last updated. Pick a smaller/faster model (a "mini", "flash-lite", or "haiku" variant) for the snapshot analysis if you want near-instant alerts, and a larger model for the clip analysis if you want richer detail. Custom model IDs are also accepted if your provider adds something new later.

## Contributing

Bug reports and pull requests are welcome — please include your Home Assistant/Frigate/LLM Vision versions and a minimal config that reproduces the issue.

---

**Version:** v0.8 · **Author:** whag · **Compatibility:** Home Assistant 2024.1+, Frigate 0.13+

## Quick Links

- [Blueprint file](blueprints/automation/frigate_llm_notification_v0_8.yaml) – ready to import
- [Alternate AI Task blueprint](blueprints/automation/frigate_ai_task_notification.yaml) – same idea, built on HA's native AI Task feature instead of LLM Vision
- [Blueprint notes / changelog](docs/blueprint-notes.md)
- [Frigate documentation](https://docs.frigate.video/)
- [LLM Vision setup](https://github.com/valentinfrlch/ha-llmvision)
- [Home Assistant Community](https://community.home-assistant.io/)
