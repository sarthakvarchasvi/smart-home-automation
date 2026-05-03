# 🏠 Distributed Smart Home Automation — Local-First Architecture

> Security-first · Fully local · Ongoing personal project

[![Home Assistant](https://img.shields.io/badge/Home_Assistant-41BDF5?style=flat-square&logo=homeassistant&logoColor=white)]()
[![MQTT](https://img.shields.io/badge/MQTT-660066?style=flat-square)]()
[![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi-A22846?style=flat-square&logo=raspberrypi&logoColor=white)]()

> ⚠️ **Note:** Security-sensitive configuration files are not published. This repository documents the architecture and design decisions.

---

## Overview

A fully local smart home automation system running on a Raspberry Pi — designed around a core principle: **the house should work when the internet is down, and sensitive data should never leave the local network.**

Started 2023 — still active and expanding.

---

## Architecture

```
Internet
    │
    │ (Only ThingSpeak module exposed)
    │
┌───▼──────────────────────────────────────────────┐
│              Home Network (LAN)                   │
│                                                   │
│  ┌─────────────────┐    ┌──────────────────────┐ │
│  │  Raspberry Pi   │    │   Control Nodes      │ │
│  │  Home Assistant │◄──►│   ESP32 / Arduino    │ │
│  │  MQTT Broker    │    │   Sensor endpoints   │ │
│  └────────┬────────┘    └──────────────────────┘ │
│           │                                       │
│           ▼                                       │
│  ┌─────────────────┐                             │
│  │  ThingSpeak      │ ◄── Only external exposure  │
│  │  (cloud API)     │     (non-sensitive data)    │
│  └─────────────────┘                             │
└──────────────────────────────────────────────────┘
```

---

## Design Philosophy

Most smart home systems have everything in the cloud — Amazon, Google, Samsung servers hold your device state. This has two problems: it stops working when internet goes down, and it means a third party always knows when you're home, when lights go on, when motion is detected.

This system inverts that: **everything runs locally by default.** The Raspberry Pi is the brain. MQTT is the communication bus. Home Assistant is the UI and automation engine. None of this requires internet to function.

The only internet-facing component is ThingSpeak — used for logging selected sensor data (temperature, humidity) that genuinely has no privacy implications. All control functions are LAN-only.

---

## Integration Points

- **Alexa** — local integration via Home Assistant's Alexa component (no cloud skill, direct LAN)
- **ThingSpeak** — selective telemetry for non-sensitive environmental data
- **Local sensors** — temperature, humidity, motion, door contact
- **Relay control** — ESP32/Arduino nodes controlling appliances over MQTT

---

## Status

🟢 Active — running continuously since 2023  
Expanding: exploring Thread/Matter protocol integration for new devices
