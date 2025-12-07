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
