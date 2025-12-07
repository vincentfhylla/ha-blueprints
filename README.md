# 🪟 Smart Window Master Controller (Unified)

### **Automate up to 12+ motorized windows using temperature, weather, air quality, presence, and safety sensors.**

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Blueprint-41BDF5)
![Status](https://img.shields.io/badge/Status-Stable-brightgreen)

---

## 🚀 Overview

This blueprint provides a **central controller** for any home with multiple motorized windows (e.g., Zooz ZEN52 DC motor controllers).  
It intelligently syncs all windows and coordinates cooling, weather protection, cooking ventilation, smoke emergency egress, AC shutdown, fault safety, and geofencing — all from one automation.

Perfect for whole-home smart window systems with 4–20+ motorized windows.

---

## ✨ Features

### 🔥 Smoke / CO Emergency Override  
If a smoke or CO alarm triggers, **all windows open immediately**, HVAC shuts off, and fans turn off.  
Designed for motorized windows without manual cranks.

### 🌡️ Natural Cooling  
Automatically opens windows when the outside temperature is cooler than inside by a configurable delta.  
Disables AC to avoid fighting your cooling strategy.

### 🍳 Cooking Ventilation Mode  
Triggered by a **smart air-quality binary sensor** (PM2.5, CO₂, VOCs).  
Opens kitchen windows to 30% and turns on fans while disabling HVAC.

### 🌧 Weather Lock  
Detects rain or snow and closes all windows immediately.

### 🏠 Geofencing  
When everyone leaves home, all windows close and cooling mode is disabled.

### 🛡 Lockout & Safety  
Prevents movement when:
- A window contact sensor indicates obstruction  
- A motor fault group is active  
- Lockout mode is on  

### 🗂 Status Tracking  
Updates `input_select.window_command_mode` with:
idle
cooling_open
cooling_close
cooking_vent
weather_close
geo_away_close
safety_smoke
lockout

---

## 🧩 Required Helpers

Create these in Home Assistant:
input_select.window_command_mode
input_boolean.window_cooling_enabled
input_boolean.window_lockout_mode

## 🧩 Required Groups
group.window_openers
group.main_floor_windows
group.window_contacts
group.window_faults (optional)
group.smoke_detectors

---

## 🧪 Cooking Sensor Template (with Hysteresis)

Add to `configuration.yaml`:

```yaml
template:
  - binary_sensor:
      - name: "Cooking Air Quality Trigger"
        unique_id: cooking_air_quality_trigger
        state: >
          {% set pm  = states('sensor.kitchen_view_plus_pm2_5') | float(0) %}
          {% set co2 = states('sensor.kitchen_view_plus_carbon_dioxide') | float(0) %}
          {% set voc = states('sensor.kitchen_view_plus_volatile_organic_compounds_parts') | float(0) %}
          {% set pm_on   = 35 %}
          {% set pm_off  = 25 %}
          {% set co2_on  = 950 %}
          {% set co2_off = 800 %}
          {% set voc_on  = 300 %}
          {% set voc_off = 200 %}
          {% set was_on = is_state('binary_sensor.cooking_air_quality_trigger', 'on') %}
          {% if was_on %}
            {{ pm > pm_off or co2 > co2_off or voc > voc_off }}
          {% else %}
            {{ pm > pm_on or co2 > co2_on or voc > voc_on }}
          {% endif %}
        device_class: smoke


