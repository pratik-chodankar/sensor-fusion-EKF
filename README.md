# Multi-Sensor Fusion — Extended Kalman Filter

**MSc Robotics | University of Sheffield | 2024-2025**

---

## What This Project Does

Implements an Extended Kalman Filter (EKF) that fuses data from multiple sensors to estimate the position, velocity, and attitude of a 6-DOF aerial platform in real time.

The filter combines:
- **IMU** (accelerometer and gyroscope) — high frequency, drifts over time
- **GPS** — low frequency, accurate position but noisy
- **Air-data sensors** — airspeed and barometric altitude

By fusing all three, the system maintains accurate state estimates even when individual sensors fail or degrade.

---

## Key Results

| Test Condition | Outcome |
|---|---|
| Clean sensor data | Filter converges within 2 seconds |
| GPS dropout (simulated) | Position estimate maintained using IMU integration |
| IMU bias injection | Detected and flagged by FDI module |
| High noise conditions | Stable estimates maintained across all 15+ test cases |
| Sensor configurations tested | 15+ |

---

## Tech Stack

- MATLAB R2024b
- Python 3
- Extended Kalman Filter (EKF)
- Fault Detection and Isolation (FDI)
- IMU, GPS, air-data sensor models
- Gazebo simulation (validation environment)

---

## Repository Structure

```
sensor-fusion-ekf/
├── ekf_core/
│   ├── ekf_main.m               - Main EKF implementation
│   ├── predict_step.m           - State prediction using IMU
│   ├── update_step.m            - Measurement update (GPS, air-data)
│   └── sensor_models.m         - Noise and bias models for each sensor
├── fault_detection/
│   ├── fdi_module.m             - Fault detection and isolation logic
│   ├── dropout_handler.m        - GPS dropout response
│   └── bias_detector.m         - IMU bias drift detection
├── validation/
│   ├── test_clean.m             - Baseline test, clean sensor data
│   ├── test_gps_dropout.m       - GPS failure scenario
│   ├── test_imu_bias.m          - IMU bias injection scenario
│   └── test_high_noise.m        - High noise scenario
├── results/
│   ├── filter_convergence.png   - EKF convergence plot
│   ├── position_estimate.png    - Position accuracy vs ground truth
│   └── fdi_response.png         - Fault detection trigger events
└── README.md
```

---

## How to Run

**MATLAB:**
1. Open MATLAB R2024b
2. Add the project folder to path:
   ```matlab
   addpath(genpath('sensor-fusion-ekf'))
   ```
3. Run a test scenario:
   ```matlab
   test_clean          % baseline test
   test_gps_dropout    % GPS failure test
   test_imu_bias       % bias injection test
   ```

**Python:**
```bash
cd sensor-fusion-ekf/python
pip install numpy scipy matplotlib
python ekf_main.py
```

---

## How It Works

### Prediction Step
Uses IMU measurements (accelerometer and gyroscope) to propagate the state forward in time. IMU data arrives at high frequency (100+ Hz) giving smooth short-term estimates. Over time, IMU integration accumulates drift.

### Update Step
When a GPS or air-data measurement arrives, the filter corrects its prediction. The Kalman gain determines how much to trust the sensor measurement vs the prediction, based on estimated noise levels.

### Fault Detection and Isolation
The FDI module monitors residuals between predicted and measured values. When a residual exceeds a threshold, the sensor is flagged as potentially faulty. The filter continues running using remaining healthy sensors.

---

## Applications

This type of sensor fusion is used in:
- Autonomous drone navigation
- Ground robot localisation (combined with Nav2 in ROS 2)
- Self-driving vehicles
- Industrial autonomous mobile robots (AMRs)

---

## Related

- [swarm-robotics-kilobot](https://github.com/33hacker33/swarm-robotics-kilobot) — MSc dissertation project
- LinkedIn: [pratik-chodankar200114](https://www.linkedin.com/in/pratik-chodankar200114)
