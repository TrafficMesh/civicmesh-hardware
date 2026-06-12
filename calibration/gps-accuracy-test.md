# GPS Accuracy Testing

## u-blox NEO-M9N Validation

- Horizontal Accuracy: <2.5m (95th percentile)
- Vertical Accuracy: <4m
- Cold Start: ~45s
- Hot Start: ~5s
- Update Rate: 10Hz

## Test Procedure
1. Deploy in OC Transpo bus for 1 week
2. Log GPS fixes every second
3. Compare against ground truth
4. Calculate accuracy statistics
