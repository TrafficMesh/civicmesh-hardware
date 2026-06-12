# CivicMesh Hardware

Node hardware design, BOMs, and specifications for Phase 0 OC Transpo pilot.

## Directory Structure

```
civicmesh-hardware/
├── bom/
│   ├── NODE_BOM.md              # Per-node cost breakdown ($511/unit, $25,550 for 50 units)
│   ├── DASHCAM_SPECS.md         # Viofo A119: 1440p @ 30fps, 120° FoV, H.264 encoding
│   ├── POWER_MANAGEMENT.md      # OBD-II 12V → 5V buck, Li-ion battery backup (15-18hr)
│   └── component-pricing.csv    # Cost tracking (parsed by GitHub Actions)
├── schematics/                  # (Phase 1+) KiCad or PDF schematics
│   └── power-distribution.kicad # Power distribution board schematic
├── firmware-requirements/
│   ├── gpio-pinout.md           # RPi CM4 GPIO assignments (camera, OBD, GPS, modem, power)
│   ├── usb-layout.md            # USB hub ports: dashcam, modem, spare, spare
│   └── thermal-profile.md       # Operating range (-10°C to +60°C), heat dissipation
├── calibration/
│   ├── camera-intrinsics.yaml   # Lens distortion coefficients (focal length, principal point)
│   ├── gps-accuracy-test.md     # u-blox NEO-M9N validation (<2.5m horizontal accuracy)
│   └── obd-ii-can-protocol.md   # CAN bus reference (500K baudrate, relevant PIDs)
├── assembly/
│   ├── ASSEMBLY_GUIDE.md        # Step-by-step node build (30 minutes per unit)
│   ├── QA_CHECKLIST.md          # Power-on, thermal, weatherproofing, connectivity tests
│   └── TROUBLESHOOTING.md       # Common issues: camera, OBD-II, GPS, modem
├── cost-tracking/
│   └── phase-0-cost-analysis.csv  # BOM cost trends over time (unit cost, labor, margins)
├── .github/workflows/
│   └── bom-cost-tracking.yml    # Parse BOM, track costs, alert on threshold changes
├── docs/
│   ├── PHASE_0_MVP_ARCHITECTURE.md    # Node specs, power budget, thermal analysis
│   └── PHASE_0_COMPLETE_DELIVERY.md   # Delivery checklist (assembly, QA, field testing)
└── README.md                    # This file
```

## Hardware Bill of Materials

### Per-Unit Cost: $511 (Revenue: $664 @ 30% margin)

| Component | Qty | Unit Cost | Subtotal | Supplier |
|-----------|-----|-----------|----------|----------|
| Raspberry Pi CM4 2GB eMMC | 1 | $55 | $55 | RS Components |
| Carrier Board | 1 | $45 | $45 | Custom |
| Viofo A119 Dashcam | 1 | $250 | $250 | Amazon |
| Quectel EC25 LTE Modem | 1 | $35 | $35 | AliExpress |
| u-blox NEO-M9N GPS | 1 | $30 | $30 | Digi-Key |
| TPM 2.0 Module | 1 | $20 | $20 | Adafruit |
| OBD-II Interface | 1 | $25 | $25 | eBay |
| Power Management | 1 | $40 | $40 | Custom |
| Enclosure + Weatherproofing | 1 | $11 | $11 | Amazon |

**Total BOM: $511/unit**  
**50-unit COGS: $25,550**

## Node Specification

### Compute & Storage
- **Processor:** Raspberry Pi CM4 2GB eMMC
- **Storage:** 128GB microSD card

### Sensors
- **Camera:** Viofo A119 (1440p @ 30fps, 120° FoV)
- **GPS:** u-blox NEO-M9N (<2.5m accuracy, 10Hz update)
- **OBD-II:** Quectel EC25 CAN interface (500K baudrate)
- **Vehicle Speed:** OBD-II PID 0x0D via CAN

### Connectivity
- **Cellular:** Quectel EC25 LTE modem
- **WiFi:** Optional (via USB hub)
- **GPS:** u-blox UART communication

### Power Management
- **Input:** OBD-II 12V from vehicle
- **Buck Converter:** 12V → 5V @ 3A, <65°C under load
- **Battery Backup:** Li-ion UPS (10Ah, 15-18hr runtime)
- **Auto-Switchover:** <5ms on power loss
- **Fusing:** 10A input, 5A output

### Security
- **TPM 2.0:** Hardware security module
- **HMAC-SHA256:** Node signature verification
- **Encryption:** TLS 1.3 for uplink

## Assembly & Testing

### Assembly Time: 30 minutes per node

**Steps:**
1. Install Raspberry Pi CM4 into carrier board SO-DIMM slot
2. Mount USB hub and connect peripherals (camera, modem, OBD-II, GPS)
3. Install buck converter and battery backup module
4. Connect power distribution (OBD-II input, RPi/modem outputs)
5. Place board in weatherproof enclosure with thermal interface material
6. Apply tamper-evident stickers

See `assembly/ASSEMBLY_GUIDE.md` for detailed procedures.

### QA Testing: Power-On, Thermal, Connectivity

**Power-On Test**
- [ ] Node powers on from OBD-II input
- [ ] LED indicators light up
- [ ] No smoke or burning smell

**Thermal Test**
- [ ] Idle temperature: <40°C
- [ ] Full load temperature: <60°C
- [ ] No thermal throttling

**Connectivity Test**
- [ ] Camera streams video (GStreamer)
- [ ] OBD-II reads vehicle speed
- [ ] GPS gets fix within 2 minutes
- [ ] LTE modem registers on network

See `assembly/QA_CHECKLIST.md` for complete checklist.

## Calibration

### Camera Intrinsics
- Focal length, principal point, distortion coefficients
- OpenCV calibration with checkerboard
- See `calibration/camera-intrinsics.yaml`

### Geospatial Extrinsics
- Map pixel coordinates to real-world 3D locations
- GPS-anchored reference frames
- Used for lane detection accuracy

### GPS Accuracy Validation
- <2.5m horizontal (95th percentile)
- <4m vertical
- Cold start ~45s, hot start ~5s
- See `calibration/gps-accuracy-test.md`

## Documentation

- [Node BOM](bom/NODE_BOM.md) - Cost breakdown and sourcing
- [Dashcam Specs](bom/DASHCAM_SPECS.md) - Camera specifications and calibration
- [Power Management](bom/POWER_MANAGEMENT.md) - Power design, battery backup, protection
- [GPIO Pinout](firmware-requirements/gpio-pinout.md) - RPi CM4 pin assignments
- [Assembly Guide](assembly/ASSEMBLY_GUIDE.md) - Step-by-step build procedure
- [QA Checklist](assembly/QA_CHECKLIST.md) - Testing procedures
- [Troubleshooting](assembly/TROUBLESHOOTING.md) - Common issues and fixes
- [Central Docs Hub](https://github.com/TrafficMesh/civicmesh-docs) - Architecture, compliance, roadmap

## Phase 0 Success Criteria

- [x] Hardware specification and BOM
- [ ] Component sourcing (50 units)
- [ ] Assembly procedures documented
- [ ] QA testing procedures
- [ ] 50 units assembled and tested
- [ ] Field deployment (5-10 buses)
- [ ] Cost tracking and analysis

## Cost Tracking

Cost data is tracked in `cost-tracking/phase-0-cost-analysis.csv` and automatically parsed by GitHub Actions workflow `bom-cost-tracking.yml`.

**Alerts:** Notified if unit cost changes >5% or margin drops below 25%.

## Contributing

See [CONTRIBUTING.md](https://github.com/TrafficMesh/civicmesh-docs/blob/main/community/CONTRIBUTING.md) in the docs repository.

## Team

- **Owner:** @TrafficMesh/hardware-team
- **Contacts:** Hardware Tech Lead

## License

Dual license: AGPL (open source) + BSL (commercial)
