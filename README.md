# ESP32 Real-Time Patient Health Monitoring System

A comprehensive IoT-based patient health monitoring system built with ESP32 microcontroller, integrating multiple health sensors for real-time vital signs monitoring, GPS tracking, and emergency alert system.

## 📋 Project Overview

This system continuously monitors patient vital signs including:
- **Heart Rate & Blood Oxygen (SpO2)** via MAX30102
- **ECG (Electrocardiogram)** via AD8232
- **Body Temperature** via MLX90614 (Non-contact IR sensor)
- **Fall Detection** via MPU6050 (Accelerometer/Gyroscope)
- **GPS Location Tracking** via NEO-6M module
- **Emergency Alerts** via Buzzer & LED indicators
- **Real-time Display** via I2C LCD Display

## 🛠️ Hardware Components

| Component | Model | Quantity | Purpose |
|-----------|-------|----------|---------|
| Microcontroller | ESP32 | 1 | Main processing unit with WiFi/BLE |
| Pulse Oximeter | MAX30102 | 1 | Heart rate & SpO2 measurement |
| ECG Module | AD8232 | 1 | Electrocardiogram signal acquisition |
| IR Temperature | MLX90614 | 1 | Non-contact body temperature |
| IMU Sensor | MPU6050 | 1 | Fall detection & motion tracking |
| GPS Module | NEO-6M | 1 | Location tracking |
| Charging Module | TP4056 | 1 | Lithium battery charging |
| Battery | 3.7V Lithium-Ion | 1 | Power supply (2000-3000 mAh recommended) |
| Display | 16x2 I2C LCD | 1 | Real-time data display |
| Alert | Buzzer (5V) | 1 | Emergency alarm |
| Status | LED (Red/Blue) | 2 | System status indicators |
| Breadboard | 830-pin | 1 | Circuit prototyping |
| Connectors | Jumper wires | 40+ | Interconnections |

## 🔌 Pin Configuration

### ESP32 Pin Assignments

```
MAX30102 (I2C - 0x57):
  SDA → GPIO 21 (I2C SDA)
  SCL → GPIO 22 (I2C SCL)
  
AD8232 (ECG):
  LO+ → GPIO 34 (ADC Input)
  LO- → GPIO 35 (ADC Input)
  OUTPUT → GPIO 32 (ADC Input)
  
MLX90614 (I2C - 0x5A):
  SDA → GPIO 21 (I2C SDA) [Shared with MAX30102]
  SCL → GPIO 22 (I2C SCL) [Shared with MAX30102]
  
MPU6050 (I2C - 0x68):
  SDA → GPIO 21 (I2C SDA) [Shared]
  SCL → GPIO 22 (I2C SCL) [Shared]
  INT → GPIO 25 (Interrupt)
  
NEO-6M (UART):
  TX → GPIO 17 (RX)
  RX → GPIO 16 (TX)
  
LCD Display (I2C - 0x27):
  SDA → GPIO 21 (I2C SDA) [Shared]
  SCL → GPIO 22 (I2C SCL) [Shared]
  
Buzzer:
  Signal → GPIO 26
  GND → GND
  
LED (Red - Alert):
  Anode → GPIO 27 (via 220Ω resistor)
  Cathode → GND
  
LED (Blue - Normal):
  Anode → GPIO 14 (via 220Ω resistor)
  Cathode → GND
```

## ⚙️ Required Libraries

Install the following Arduino libraries via Library Manager:

```
1. MAX30105 Library by sparkfun
2. Heart Rate Calculation by sparkfun
3. Adafruit MLX90614 Library
4. MPU6050 - I2Cdev by Jeff Rowberg
5. LiquidCrystal_I2C by Frank de Brabander
6. TinyGPSPlus by Mikal Hart
7. ArduinoJson by Benoit Blanchon
```

## 📦 Installation Steps

1. **Setup Arduino IDE:**
   - Install ESP32 board support
   - Configure serial port and upload speed (115200 baud)

2. **Install Required Libraries:**
   ```
   Sketch → Include Library → Manage Libraries
   Search and install the libraries listed above
   ```

3. **Configure WiFi & Cloud:**
   - Update WiFi credentials in `config.h`
   - Set up cloud endpoint (ThingSpeak, Firebase, etc.)

4. **Upload Code:**
   - Connect ESP32 via USB
   - Select Board: "ESP32 Dev Module"
   - Upload `main_health_monitor.ino`

## 📊 System Architecture

```
┌─────────────────────────────────────────────┐
│         SENSOR DATA ACQUISITION              │
├─────────────────────────────────────────────┤
│ MAX30102 → Heart Rate, SpO2                  │
│ AD8232 → ECG Signal                          │
│ MLX90614 → Temperature                       │
│ MPU6050 → Acceleration (Fall Detection)      │
│ NEO-6M → GPS Coordinates                     │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
        ┌─────────────────────┐
        │   ESP32 Processor   │
        │  - Data Processing  │
        │  - Alert Logic      │
        │  - Display Update   │
        └────────┬────────────┘
                 │
        ┌────────┴────────┐
        │                 │
        ▼                 ▼
    LOCAL DISPLAY    CLOUD UPLOAD
    (I2C LCD)        (WiFi)
        │                 │
        ├─────────────────┤
        │   Alerts & LEDs │
        │   (Buzzer/LED)  │
        └─────────────────┘
```

## 🚀 Features

✅ **Real-time Vital Signs Monitoring** - Continuous heart rate, SpO2, temp tracking
✅ **ECG Signal Acquisition** - Raw ECG data for cardiac analysis
✅ **Automatic Fall Detection** - IMU-based fall alerts with buzzer
✅ **GPS Tracking** - Real-time location logging
✅ **Multi-sensor I2C Bus** - Efficient sensor communication
✅ **Local LCD Display** - Live data visualization
✅ **Cloud Integration** - Remote monitoring capability
✅ **Battery Powered** - Lithium-ion with charging module
✅ **Emergency Alerts** - Visual (LED) and audio (buzzer) warnings
✅ **Low Power Mode** - Sleep management for extended battery life

## 📱 Data Display Format (LCD)

```
Line 1: HR:75 SPO2:98% T:36.5C
Line 2: FallAlert: NO GPS:Ready
```

## 🚨 Alert Conditions

| Condition | Action | Trigger |
|-----------|--------|---------|
| Heart Rate < 50 BPM | Red LED + Buzzer | HR anomaly |
| SpO2 < 90% | Red LED + Buzzer | Low oxygen |
| Temperature > 39°C | Red LED + Buzzer | Fever |
| Fall Detected | Red LED + Buzzer | Acceleration spike |
| All Normal | Blue LED | System OK |

## 🔋 Power Management

- **Input**: 5V via USB or TP4056 charging module
- **Battery**: 3.7V Lithium-Ion (2000-3000 mAh)
- **Charging**: TP4056 with micro-USB input
- **Runtime**: 8-12 hours (continuous monitoring)
- **Sleep Mode**: ~20 mA (in development)

## 🌐 Cloud Integration

Supports integration with:
- **ThingSpeak** - Free IoT platform with visualization
- **Firebase Realtime Database** - Real-time sync
- **MQTT Broker** - IoT standard protocol
- **REST API** - Custom backend

## 📈 Data Logging

Data is logged with timestamp:
```json
{
  "timestamp": "2026-09-27T10:30:45Z",
  "heart_rate": 75,
  "spo2": 98,
  "temperature": 36.5,
  "gps_lat": 13.1939,
  "gps_lon": 77.6245,
  "fall_detected": false,
  "alerts": []
}
```

## 🔒 Security Considerations

- Use HTTPS/TLS for cloud communication
- Store WiFi credentials securely
- Implement authentication for remote monitoring
- Encrypt sensitive patient data
- Regular firmware updates

## 📝 Code Structure

```
├── main_health_monitor.ino     # Main program
├── config.h                    # Configuration & credentials
├── sensors/
│   ├── max30102_handler.cpp    # Heart rate/SpO2
│   ├── ad8232_handler.cpp      # ECG signal
│   ├── mlx90614_handler.cpp    # Temperature
│   ├── mpu6050_handler.cpp     # Fall detection
│   └── neo6m_handler.cpp       # GPS tracking
├── display/
│   └── lcd_handler.cpp         # I2C LCD display
├── alerts/
│   └── alert_system.cpp        # Buzzer & LED control
└── cloud/
    └── cloud_sync.cpp          # WiFi & data upload
```

## 🧪 Testing Procedures

1. **Sensor Calibration**: Calibrate each sensor individually
2. **I2C Address Detection**: Scan I2C bus for connected devices
3. **Signal Quality**: Verify clean sensor signals
4. **Fall Detection**: Test IMU thresholds
5. **GPS Lock Time**: Note initial fix time (~30-60s)
6. **Battery Life**: Monitor under continuous operation

## 📚 Documentation

- Detailed wiring diagram in `docs/circuit_diagram.md`
- Sensor datasheets in `docs/datasheets/`
- API documentation in `docs/api.md`
- Troubleshooting guide in `docs/troubleshooting.md`

## 🐛 Troubleshooting

**Issue**: MAX30102 not detected
- Check I2C address (0x57)
- Verify SDA/SCL connections to GPIO 21/22
- Use I2C scanner sketch to debug

**Issue**: ECG signal noisy
- Ensure proper electrode placement
- Check AD8232 LO- cable connection
- Add capacitive filtering

**Issue**: GPS not acquiring fix
- Wait 30-60 seconds for initial lock
- Ensure GPS antenna has clear sky view
- Check UART connection (GPIO 16/17)

**Issue**: Fall detection too sensitive
- Adjust MPU6050 threshold in code (lines 45-50)
- Calibrate on flat surface

## 📄 License

MIT License - See LICENSE file for details

## 👥 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create feature branch (`git checkout -b feature/YourFeature`)
3. Commit changes (`git commit -m 'Add YourFeature'`)
4. Push to branch (`git push origin feature/YourFeature`)
5. Open Pull Request

## 📞 Support & Contact

For issues, questions, or suggestions:
- Open an issue on GitHub
- Check existing documentation
- Review sensor datasheets

## 🙏 Acknowledgments

- Sparkfun for MAX30102 library
- Adafruit for MLX90614 library
- Jeff Rowberg for MPU6050 I2Cdev library
- All open-source contributors

---

**Last Updated**: September 2026
**Status**: Active Development
**Version**: 1.0.0
