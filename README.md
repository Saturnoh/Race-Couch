# Race Couch!

### What is Race Couch?

Race Couch (as shown here) is version 4 of a concept that has been floating around in Wreck Racing for a couple years. 

<img width="1278" height="959" alt="image" src="https://github.com/user-attachments/assets/3d6c9336-5510-4279-8f24-bae73214f38c" />


## 🧠 System Architecture

The control system is built around an ESP32 microcontroller, interfacing with custom input hardware and dual motor controllers.

| Component | Description & Function |
| :--- | :--- |
| **Inland ESP32-WROOM-32** | The central brain. Handles driver inputs, signals motor controllers via DACs, and manages the precharge relay switch. |
| **Kelly Motor Controllers** | Inverts 48V DC battery power to 3-phase AC. Interprets hall effect signals for position data and modulates motor speed based on a 0-5V analog input. |
| **I2C DACs** | Controlled by the ESP32 to translate digital throttle inputs into the 0-5V analog signals required by the Kelly controllers. |
| **48V Brushless Hub Motors** | DIY electric scooter motors mounted to custom steel wheel brackets. Drives the vehicle using 3-phase power. |

## 🔋 Power Delivery & Safety

Because of the high capacitance in the motor controllers, direct connection to the 48V battery would cause a massive inrush current, damaging the components. The power system is designed around a safe startup sequence.

*   **Precharge Circuit:** A solid-state relay (controlled by the ESP32) temporarily routes power through a precharge resistor. This slowly brings the motor controllers' B+ bus up to battery voltage during startup.
*   **Main Power Killswitch:** The primary flow of battery power once precharge is complete. Doubles as an emergency cut-off to kill all power in a hazard scenario.
*   **Fusebox:** Sits between the 48V main line and each motor controller to protect against high current or arcing.
*   **Buck Converters:** Steps down 48V to 12V (for the motor controller enable signals) and 12V to 5V. *(Note: The ESP32 is currently powered via a USB power bank due to undervoltage issues on the 5V rail).*
*   **Mechanical Brakes:** A fail-safe and parking brake utilizing mechanical bicycle cables connected to DIY scooter disc brakes on the front wheels.

## 🚀 Improvements Made

The vehicle's steering and power systems have recently been overhauled from their original iterations:

1.  **Physical Steering System:** Replaced the original PlayStation 4 Bluetooth controller (which relied on complex tank-steering that conflicted with motor braking) with a dedicated, physical steering wheel for intuitive control.
2.  **Automated Main Contactor:** Upgraded the relay hardware to a main contactor. The precharge sequence and switch to main power are now fully automated, replacing the old manual switchover process.

## ⚠️ Known Issues

*   **Weight-to-Torque Ratio:** The heavy weight of the furniture results in relatively slow acceleration.
*   **Overcurrent Risks:** The motors can draw too much current and blow fuses if the power curves are set too aggressively in the Kelly tuning software.
*   **Bluetooth Drops:** (Legacy issue) The original PS4 controller would occasionally lose connection during the precharge sequence, though it remained stable while driving. 
*   **Lack of E-Braking:** Due to earlier issues with the tank-steering configuration, regenerative/electronic braking is currently disabled.

## 🗺️ Future Roadmap

*   **Braking Overhaul:** Develop an easier, more intuitive electronic braking system using the motor controllers.
*   **Power Curve Smoothing:** Refine the acceleration curves in the tuning software for a smoother, more enjoyable driving experience. 
*   **Hardware Consolidation:** Design a centralized junction box to clean up the wiring harness.
*   **Dashboard Integration:** Hard-mount the e-stop and motor controller toggle switches in a permanent, easily accessible location for the driver.
*   **E-Stop Sequence:** Wire the new main contactor in series with the e-stop, allowing the user to enable it once before initiating the fully automated power-on process.


## Wiring Diagram
<img width="1341" height="789" alt="image" src="https://github.com/user-attachments/assets/75e369b1-c01e-441a-aaf1-a5586ba2ae98" />
