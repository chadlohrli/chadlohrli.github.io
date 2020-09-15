---
title: "Custom Quadcopter"
summary: "Built with Eagle CAD"
---

{% assign image_path = '/assets/images/quadcopter' %}

### Background

This was an elective course I took at UCSD which ended up being very beneficial for my hardware career.
I had been playing around with arduinos and various IoT devices for some time
and this was a good opportunity to apply my bread board knowledge to the work of PCBs.

---

Tools used:
* EAGLE CAD
* Arduino IDE

---

#### The Schematic
<img src="{{site.url}}{{image_path}}/schematic.png" width="500" height="500" />    
__Sections:__
* Microcontroller
* Motor Controllers
* IMU
* Power Supply
* Debug Headers
* Programming (FTDI) Headers
* LED's    

<br/>__Main Parts:__
* ATmega128RFA1 (MCU)
* ANT-2.45-CHP-x (Antenna)
* LSM9DS1 (IMU)
* Coreless Brushed Motors   

---

#### The Board
<img src="{{site.url}}{{image_path}}/board.png" width="500" height="500" />   

Before acutally laying out the board, a few of the SMD components (capacitors & LEDS) needed custom packages which were created based on their datasheet specifications.

Designing the board in EAGLE was fun and intuitive. Being able to build multiple layers, this one had 4, 
made it a lot easier for the autorouter to create all the connections. Minor tweaking involved.

Placing the SMD components on the manufactured PCB by hand and reflow soldering was not as fun nor easy but nonetheless a good learning experience. In the future I would definitely pay for a turnkey solution.

---

#### The Final Product
<img src="{{site.url}}{{image_path}}/final.jpg" width="500" height="500" />   
The end product ended flying but had some difficulty staying level due to the PID code

---

#### Code Snippets   
<br/>**Complementary Filter**   
To account for the noise in the IMU created by the motors, a complementary filter can be used for correction. To correct the pitch, a weighted sum of the accelerometer and integral of gyroscope can be used.

```C++
  ...
  float comp_pitch = (1.0-gain)* (pitch_rate * dt + prev_pitch) + (gain)*(pitch);
  prev_pitch = pitch;
```

<br/>**PID Controller**   
A PID (proportional-integral-derivative) controller is used to level the quadcopter after the pitch has changed. 
<img src="https://hackernoon.com/hn-images/1*h-e5l4pzsM1_fsETVIsZJA.png" width="250" height="100" />   
```C++
  ...
  p_error = error;
  error = 0.0 - compPitch;
  pid = kp*error + kd*(error - p_error)/dt + ki*(error * dt);
```

---

### Conclusion
The quadcopter was operational in the end but had some trouble flying. This was in part due to the low effort in tweaking the complementary and PID parameters and spending most of the time on the hardware.

The big takeaway was the ability to create a functional PCB from start to finish, a skill I still use today