# CLAUDE.md

This file provides guidance for AI assistants working with this repository.

## Project Overview

This is the companion code repository for the Packt book **"Building Smart Home Automation Solutions with Home Assistant"** by Marco Carvalho. It contains configuration files, automation examples, device firmware dumps, Node-RED flows, Grafana dashboards, and tutorial videos for building a smart home automation system.

**License:** MIT (Copyright 2023 Packt)

This is an **educational/reference repository**, not a production application. Files are example configurations and tutorials organized by book chapter.

## Repository Structure

```
Chapter 01/   - Fundamental concepts (theory, no code)
Chapter 02/   - Fundamental concepts continued (theory, no code)
Chapter 03/   - Build your own IoT sensor (ESP8266/ESP32 firmware dumps, videos)
Chapter 04/   - Configure actuators (Tasmota firmware dumps, videos)
Chapter 05/   - Automations & blueprints (YAML automations, scripts, scenes)
Chapter 06/   - Dashboards & visualizations (YAML dashboard configs, videos)
Chapter 07/   - Node-RED integration (JSON flows, videos)
Chapter 08/   - Docker Compose full-stack setup (docker-compose.yml)
Chapter 09/   - Advanced features: WLED, LED strips (JSON configs, videos)
Chapter 10/   - Data logging & visualization: InfluxDB + Grafana (YAML, JSON, videos)
Chapter 11/   - ChatGPT / AI integration (YAML automation)
```

## Key Technologies

| Technology | Role |
|---|---|
| Home Assistant | Central home automation hub |
| MQTT (Mosquitto) | Message broker for device communication |
| Node-RED | Visual flow-based automation |
| Tasmota | Open-source firmware for ESP8266/ESP32 devices |
| InfluxDB 1.8 | Time-series database for sensor data |
| Grafana | Data visualization dashboards |
| Chronograf | InfluxDB monitoring |
| TasmoAdmin | Tasmota device management |
| Portainer CE | Docker container management UI |
| Duck DNS | Dynamic DNS for remote access |
| WLED | LED strip control firmware |
| Docker Compose (v3.6) | Container orchestration for the full stack |

**Hardware:** Raspberry Pi 4, ESP8266, ESP32, various smart home devices (lights, switches, PIR sensors, temperature sensors).

## File Types and Languages

- **YAML** (.yaml) - Home Assistant configurations, automations, blueprints, dashboards, scripts, scenes
- **JSON** (.json) - Node-RED flows, Grafana dashboards, WLED LED strip configs
- **Docker Compose** (docker-compose.yml) - Full microservices stack definition
- **Tasmota firmware dumps** (.dmp) - Binary device configuration backups
- **MP4 videos** (.mp4/.MP4) - Tutorial recordings (~35 files, ~350 MB total)

## Architecture

The Docker Compose stack in Chapter 08 defines the full home automation architecture:

```
Docker Host (Raspberry Pi 4)
├── home_assistant    (port: host network)  - Main automation hub
├── mosquitto         (port: 1883)          - MQTT broker
├── nodered           (port: 1880)          - Flow automation
├── influxdb          (port: 8086)          - Time-series database
├── grafana           (port: 3000)          - Dashboards
├── chronograf        (port: 8888)          - InfluxDB monitoring
├── tasmoadmin        (port: 8088)          - Device management
├── portainer-ce      (port: 9000/9443)     - Container management
└── duckdns           (host network)        - Dynamic DNS
```

## Conventions

### Home Assistant YAML

- **Automations** follow the standard HA structure: `id`, `alias`, `description`, `trigger`, `condition`, `action`
- **Blueprints** use the `blueprint:` key with `input:` parameters for reusable templates
- **Entity IDs** use the format `domain.name` (e.g., `light.garagelights`, `switch.coffeemachine`)
- **Automation IDs** use Unix timestamps or numeric strings

### MQTT Topics

- Sensor data: `sensor/<device_name>/<sensor_type>` (e.g., `sensor/Garage_Temp_PIR/PIR1`)
- Tasmota commands: `cmnd/<device_name>/<command>`
- Snake_case naming with forward-slash separators

### Device Naming

- Descriptive names in snake_case: `garage_pir_sensor`, `master_bed`, `coffeemachine`
- Tasmota devices use config dump files named: `Config_<DeviceName>_<ID>_<FirmwareVersion>.dmp`

### Docker Compose

- All services use `restart: unless-stopped`
- Volumes mount to `./volumes/<service_name>/` relative paths
- Timezone set via `TZ=Etc/UTC` environment variable
- Health checks defined for critical services (Grafana, InfluxDB)

## Development Notes

- **No build system:** No package.json, requirements.txt, Makefile, or equivalent. This is a configuration-only repository.
- **No test framework:** No automated tests exist. Validation is done by deploying configurations to a live Home Assistant instance.
- **No CI/CD:** No GitHub Actions, Travis CI, or other pipeline configurations.
- **No linting/formatting tools:** No .editorconfig, ESLint, Prettier, or YAML linting configured.
- **No .gitignore:** The repository has no .gitignore file. Large video files are committed directly.

## Working with This Repository

### Editing YAML files

When modifying Home Assistant YAML files, ensure:
- Proper indentation (2 spaces, no tabs)
- Valid YAML syntax (use a YAML linter if available)
- Entity IDs reference actual devices in the target HA installation
- Automation triggers, conditions, and actions follow the [Home Assistant automation docs](https://www.home-assistant.io/docs/automation/)

### Editing JSON files

Node-RED flows and Grafana dashboards are JSON. When modifying:
- Validate JSON syntax before committing
- Node-RED flows use node `id` fields for internal wiring; do not change IDs without updating references
- Grafana dashboards reference InfluxDB datasources by name

### Docker Compose

The `Chapter 08/docker-compose.yml` orchestrates 9 services. To modify:
- Maintain Docker Compose v3.6 format
- Keep volume paths consistent with the `./volumes/` convention
- Do not commit secrets (passwords, tokens) - use environment variables or `docker-compose.override.yml`

## Important Considerations

1. **Sensitive data:** Some configuration files may contain device-specific IDs, IP addresses, or topic names. Do not add real credentials or tokens.
2. **Binary files:** `.dmp` firmware dumps are binary and should not be edited as text.
3. **Video files:** Large MP4 files are part of the repository. Avoid unnecessary modifications that would increase repo size.
4. **Chapter independence:** Each chapter directory is self-contained. Cross-chapter dependencies are documented in the book text, not in the repository.
