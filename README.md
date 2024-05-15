# a14g-final-submission

    * Team Number: 28
    * Team Name: SwitchLink
    * Team Members: Praveen Raj Uma Maheswari Shyam Sundar
    * Github Repository URL: https://github.com/ese5160/a14g-final-submission-t28-switchlink.git
    * Description of test hardware: A Smart Switch Control.

## 1. Video Presentation


## 2. Project Summary

### Device Description:

SwitchLink is a Smart Switch Control Unit that goes beyond conventional remote switch control. It features a Smart USB charging system utilizing an OP Amp comparator to detect the full-charge status of connected devices. This feature is especially useful for devices and appliances lacking built-in Charge Protection Circuitry. When a device reaches full charge, SwitchLink notifies the user with defined acoustic notifications generated by a Piezo-buzzer via PWM.

Key components and functionalities include:

Smart USB Charging System: Monitors and indicates full-charge status using an OP Amp comparator.

Acoustic Notifications: Utilizes a Piezo-buzzer controlled through PWM to notify users when charging is complete.

Relay Module: Enables control of electrical switches.

Proximity Sensors: Provides a non-contact user interface for switch control.

Remote Control: Allows switching control over the Internet using MQTT.

SwitchLink enhances the usability and safety of home appliances and devices, providing both manual and remote control options.


### Device Functionality of SwitchLink

SwitchLink is a sophisticated Internet-connected smart switch control unit designed to enhance the management and safety of electrical devices. Here's a detailed explanation of its design, including the sensors, actuators, and other critical components:

## Critical Components:

* Microcontroller (MCU):

SAMW25 Xplained Pro: This microcontroller handles all processing tasks, including sensor data acquisition, control logic, and communication. It is integrated with FreeRTOS for real-time task management and efficient operation.


Sensors:

* VL6180X Proximity Sensors: Two proximity sensors are used for non-contact switch control. These sensors detect the presence of a hand or object near the switch, allowing for gesture-based control of the connected devices.


* OP Amp Comparator: Used in the Smart USB charging system to detect the full-charge status of connected devices. This comparator monitors the voltage level and triggers notifications when the device is fully charged.

Actuators:

* Relay Module: Controls the switching of electrical devices. The relay module is activated based on inputs from the proximity sensors or remote commands received over the Internet.

* Piezo-buzzer: Generates acoustic notifications when a device connected to the USB charging system is fully charged. The buzzer is controlled using PWM signals to create different sound patterns.


Communication Module:

* MQTT Protocol: Enables remote control and monitoring of the switches over the Internet. The device connects to an MQTT broker, allowing users to send commands and receive status updates from anywhere with Internet access.


Design Overview:

User Interface:

Non-contact Control: The VL6180X proximity sensors provide a user-friendly, non-contact interface for controlling the switches. Users can simply wave their hand near the sensors to turn devices on or off.
Acoustic Notifications: The Piezo-buzzer offers auditory feedback when the full-charge status is detected, ensuring users are promptly notified without needing to check their devices constantly.


Smart USB Charging System:

* Charge Detection: The OP Amp comparator continuously monitors the voltage level of connected devices. When the voltage reaches a predefined threshold indicating a full charge, the comparator triggers the buzzer to notify the user.

* Safe Charging: This system is particularly useful for devices without built-in charge protection circuitry, preventing overcharging and extending battery life.


Remote Control and Monitoring:

* Internet Connectivity: The device connects to the Internet via Wi-Fi, utilizing the SAMW25's built-in capabilities. The MQTT protocol facilitates secure and reliable communication between the device and remote users.

* Mobile and Web Apps: Users can control the switches and monitor device statuses through dedicated mobile or web applications. This feature allows for convenient and flexible management of home appliances from anywhere.


![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/ac8e890e-0683-4044-8425-ab5fd9e1cf14)


Functional Workflow:
Power Up: The device initializes, and the PMIC ensures stable power distribution to all components.
Sensor Monitoring: The proximity sensors continuously monitor for user gestures to control the switches.
Charge Detection: The OP Amp comparator monitors the voltage level of connected devices.
User Interaction: When a proximity sensor detects a hand gesture, it sends a signal to the MCU, which activates the corresponding relay to turn the device on or off.
Remote Commands: Users can send commands via the Internet using the MQTT protocol, allowing remote control of the switches.
Notifications: When a device reaches full charge, the OP Amp comparator triggers the Piezo-buzzer to notify the user acoustically.

### Inspiration:

The inspiration for the SwitchLink project started from a desire to enhance the safety, convenience, and efficiency of managing household electrical devices. Traditional switch controls and basic remote systems often lack the smart features needed to prevent overcharging and improve user interaction. Here are the key factors that inspired this project:

Safety Concerns: Many devices and appliances lack built-in charge protection circuitry, which can lead to overcharging, reduced battery life, and potential safety hazards. The need to create a solution that prevents these issues was a significant motivator.

Technological Advancement: With the rise of smart homes and IoT, there was a clear opportunity to develop a system that integrates modern technology like proximity sensors and Internet-based remote control to provide a seamless and user-friendly experience.

User Convenience: Providing an easy and intuitive way to control switches without physical contact, and offering remote control capabilities via MQTT, addresses the growing demand for convenience and automation in everyday life.

Energy Efficiency: By notifying users when devices are fully charged and enabling precise control over electrical switches, SwitchLink helps in conserving energy, aligning with broader environmental goals.


### Challenges:

The VL6180X Proximity Sensor has a driver written in C++. Rewriting the whole driver in C for ASF (Atmel Software Framework) with a Shim layer to work with the I2C Driver was a challenge. Also, to verify the correctness of the driver, I carefully reviwed the extensive datasheet to find the right list of private and public registers to be accessed and address them. This process required meticulous translation of code and ensuring compatibility with the existing framework, while maintaining the sensor's functionality and performance.

![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/b4213fa0-81bb-4201-af78-0145669324ea)

In driver writing, I also faced the issue of "Register overloading" due to memory management limiations of the C language. I figured that a simple transition from sprintf() to snprintf() while writing to the registers would solve this issue as snprintf() puts a sizing bound before writing to the register.

![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/e6a7d6ef-e622-453c-904b-63c7858968c4)


Building a stable VREF source for the OP Amp comparator off-the-board was another challenge. As this reference voltage is critical for accurate full-charge detection, ensuring its stability and precision was paramount. Achieving this involved careful design and testing of the voltage reference circuitry, addressing issues such as temperature stability, noise reduction, and power supply variations.


### Prototype Learnings:

1. Importance of Thorough Testing: Rigorous testing at each stage of development is crucial. I understood the importance thorough test planning, like planning proper testpoits and debug ports. Early detection of hardware and software issues can save significant time and resources. For instance, issues with the stability of the VREF source for the OP Amp comparator highlighted the need for more extensive testing of power supply variations and environmental conditions.

2. User Interface Considerations: Designing an intuitive and responsive user interface, especially with non-contact controls like proximity sensors, requires a good understanding of user behavior and needs. Iterative testing and user feedback are vital for refining the interface.

Improvements for Future Iterations

If I had to build this device again, I would consider the following changes:

On Development Level:

1. Enhanced Testing Framework: Implement a more comprehensive testing framework from the start, including automated tests for both hardware and software components. This would facilitate early detection of issues and streamline the development process.

2. Improved Modularity: Design the system with greater modularity in mind, allowing for easier updates and replacements of individual components. This would also help in scaling the project or adapting it for different use cases.

3. Advanced User Interface: Enhance the user interface by incorporating additional feedback mechanisms, such as LEDs or an LCD display, to provide users with more information and control options.

4. Internet Disconnect Button: Worried about privacy? We'll included a designated mechanical switch that physically disconnects the system from the internet, putting the user in control of their smart home.
   
3D Render of the Future Version:

![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/1a6074bf-349f-4624-9ddb-25f4a375c782)


### Next Steps:

1. Stable Power Supply:

* Integrate advanced Power Management ICs (PMICs) to enhance power regulation and efficiency. 
* Ensure robust power sequencing and protection features are in place.


2. Component Quality:

* Source high-quality components to ensure reliability and longevity.
* Test components under various conditions to verify performance and durability.

3. Advanced Features:

*Implement additional features like advanced user notifications via mobile apps or web interfaces.
*Enhance the MQTT-based remote control functionality for more robust and secure communication.


4. User Interface:

* Improve the non-contact user interface with refined proximity sensor calibration and sensitivity adjustments.
* Add visual indicators (LEDs, LCDs) for real-time status updates and user feedback.


5. Environmental Testing:

* Test the device under various environmental conditions (temperature, humidity, etc.) to ensure reliability.
* Conduct stress tests to evaluate the device's performance under maximum load.


### Takeaways from ESE5160:

* Design, Debugging and Testing of Intricate PCBs from Component Selection, Schematic Capture, Layout design etc to Gerber file Generation,
* Concepts of FreeRTOS like Semaphores, Murex, Task Creation etc.,
  
### Project Links:

* Node-red Instance: http://172.208.48.99:1880/

* Code Repository: https://github.com/ese5160/a12g-firmware-drivers-t28-switchlink.git

* Altium Project: https://upenn-eselabs.365.altium.com/designs/0D535367-1C3A-4320-998F-776AB24CDA05



## 3. Hardware & Software Requirements

## 4. Project Photos & Screenshots

* PCB Pictures:

  ![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/eab833d5-a380-4a51-9ea8-ed61c6e9de3c)

  ![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/ee0becae-b8a0-4a12-ade9-a215dafa73a6)

  
* Thermal Evaluation of PCB Under Load:

  ![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/bcdc4a1a-68a0-4cc4-b0cc-46e487ab149e)


* Altium Board Design:

   2D: ![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/b428e645-10ee-4e0a-b56b-df39f2112e90)

   3D: ![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/05523a8c-982f-4cf0-9061-6fa94c3e1840)


* Node-red Dashboard:

   UI: ![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/941d490d-4027-4020-b007-ba4f4f53ad8c)

   Backend: ![image](https://github.com/ese5160/a14g-final-submission-t28-switchlink/assets/114270637/838307ae-c314-4298-b59e-b7c8e669e3ad)


## 5. Codebase:

Link: https://github.com/ese5160/a12g-firmware-drivers-t28-switchlink.git
