# Phase 0 MVP Hardware Architecture

## Node Specification
- Compute: Raspberry Pi CM4 2GB
- Storage: 128GB microSD
- Camera: Viofo A119 (1440p)
- Modem: Quectel EC25 LTE
- GPS: u-blox NEO-M9N
- Security: TPM 2.0

## Power Budget
- Input: 12V from OBD-II (vehicle)
- Output: 5V/3A to RPi + modem
- Runtime: 15-18 hours on battery backup
