# Troubleshooting Guide

## Camera not detected
- Check CSI-2 cable seating
- Verify in dmesg: dmesg | grep camera
- Test with libcamera-hello

## OBD-II not responding
- Check UART0 baud rate (38400)
- Verify vehicle is in ON position
- Test with: cat /dev/ttyAMA0

## GPS no fix
- Move outdoors, away from buildings
- Wait 5+ minutes for cold start
- Check antenna connection

## Modem not registering
- Check APN settings
- Verify SIM card is active
- Check signal strength with AT commands
