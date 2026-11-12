# Smart-Closed-Circuit-Grain-Silo
this a system that solves the problem on grain storage systems and helps the famers to scale up the grain production during the harvest
Why Existing Aeration Systems Fall Short
Direct Contact & Spoilage: Conventional aeration relies on perforated floors or rods that physically touch and disrupt the grain, leading to mechanical damage and contamination.

Air Channeling & Clogging: Forced air often bypasses the dense core through paths of least resistance, while dust quickly clogs perforated vents.

Counterproductive Rooftop Fans: Passive or poorly managed top exhaust fans frequently draw humid ambient air back into the silo due to pressure drops and gravity, worsening moisture buildup.

💡 The Solution & Key Innovations
This project replaces blunt, open-air aeration with a closed-circuit, non-contact thermal extraction system.

Feature	Traditional Aeration	This Project
Air-to-Grain Contact	❌ Direct contact through the crop	✅ Zero contact (sealed copper tube)
Core Heat Extraction	❌ Bypassed by air channeling	✅ Aluminum fins conduct heat directly from the core
Headspace Humidity	❌ Passive or manual intervention	✅ DHT11 sensor triggers 100% fan speed at RH≥70%
Fan Speed Control	❌ Basic On/Off switching	✅ Dynamic PWM control re-evaluated every 2 seconds
Clogging Risk	❌ High (dust blocks perforations)	✅ Zero clogging (solid, sealed tubing)
Power Consumption	❌ High-power industrial blowers	✅ Ultra-low power (compact 12V 5015 centrifugal blower)
⚙️ System Architecture & Technical Workflow
Hardware Bill of Materials (8 Off-the-Shelf Components)
Microcontroller: Arduino Uno / ESP32 (runs the 2-second control loop; PWM output on Pin 9)

Core Temperature Sensors: 
3× DS18B20 waterproof probes spaced across three vertical heights on a single 1-Wire bus (±0.5∘C accuracy)

Humidity Sensor: 
1× DHT11 sensor monitoring headspace relative humidity (RH)

Driver Module: IRF520 MOSFET module (solid-state valve switching 12V via 5V PWM)

Actuator: 12V DC 5015 centrifugal blower

Heat Exchanger: 
12 mm diameter, 
500 mm length sealed copper tube with 
70 mm aluminum fins

Airflow Funnel: Custom 3D-printed Venturi nozzle tapering the blower output into the copper tube

Power & Conditioning: 12V 2A DC power adapter + 
4.7 kΩ pull-up resistor for 1-Wire bus stability

Automated Control Logic (Re-Evaluated Every 2 Seconds)
Read Sensors: Polls all three DS18B20 core probes and the headspace DHT11 sensor.

Fault Check: Validates sensor readings. If a sensor fault is detected, the cycle is safely skipped and an error is logged.

Compute Metrics: Calculates average core temperature (T avg) and vertical temperature differential (ΔT).

Decide PWM Duty Cycle:

🔴 Critical Alert (RH≥70% OR ΔT≥8 ∘C OR T core ≥35 ∘C): 100% PWM (Full cooling)

🟠 Proportional Scaling (25 ∘C<T core<35 ∘C): Linear PWM ramp-up matching thermal load

🟢 Safe State (T core≤25∘C): Low idle PWM for minimal power draw

Actuate: Sends the PWM signal through the IRF520 MOSFET to drive the 12V blower, exhausting conducted core heat safely outside the silo.
