---
  title: "Ares Flight Computer"
  date: 2023-08-07T12:00:00+06:00
  featured: false
  tags: "tech"
  tranding: true
  readTime: "5 min"
  thumbnail: /images/blog/ares-flight-computer/flight-computer-photo.png
  featureImage: /images/blog/ares-flight-computer/flight-computer-photo.png
---

What you're seeing above is the result of my 2021 summer project – the Ares Flight Computer, a comprehensive PCB (Printed Circuit Board) solution for model rockets.

## What is Ares?

Ares is an all-in-one flight computer designed specifically for model rockets. It's equipped with onboard sensors to gauge orientation and has the capability to control rockets through servos and pyrotechnic channels. Additionally, Ares features RF communication capabilities via Bluetooth and data logging capabilities via micro SD. The computation is done by two STM32 microcontrollers that deal with the sensor gathering and command of the rocket. 

## How Ares works

The functionality of Ares can be segmented into three primary components: Input, Computation, and Output.

#### Input:
Ares is equipped with an IMU (Inertial Measurement Unit) that integrates an accelerometer, gyroscope, and magnetometer. This primary sensor provides Ares with data on its orientation, position, and velocity. Complementing the IMU is a pressure sensor, vital for position estimates and determining the rocket's maximum altitude. Additionally, two backup accelerometers, each connected to one of the MCUs, add additional information. While accelerometers might not match the precision of an IMU, they enhance the system's state estimation.

#### Computation:
The core of the computation process is the Navigation Computer, powered by an STM32L1. This unit collates all sensor inputs via SPI and relays a state estimate to the Flight Control Computer (FCC) using UART.

![](/images/blog/ares-flight-computer/block.jpeg)

Beyond sensors, the Navigation Computer supports Serial Wire Debug (SWD) connections (essential for programming), USB for efficient data extraction, and LED indicators that communicate the computer's status. The bridge between the two microcontrollers is a UART connection, facilitating data exchange.

#### Output:
The Flight Control Computer, true to its name, is entrusted with the task of rocket control. Its responsibilities span PWM outputs for servos (for thrust vector control), GPIO controls to manage a reaction wheel, and pyrotechnic channels to initiate parachute deployment. However, the FCC's role isn't confined to mere control; it also holds the duty of data logging. There are dual pathways for this: a flash chip connected via SPI and a micro SD card. While the rocket is airborne, the flash chip captures data in real-time. Once the rocket is in a safe state post-flight, this data is transferred to the SD card, ensuring data integrity and ease of access.

Through the sensors, MCUs, and connectivity solutions, Ares stands as a fully-functional flight computer.



<p align="center">
  <img src="/images/blog/ares-flight-computer/ares-front.PNG" alt="Alt text" width="500">
  <img src="/images/blog/ares-flight-computer/ares-back.PNG" alt="Alt text"
  width="500">
</p>

## The Making of Ares

However, my journey to design Ares was not without challenges. I embarked on this project in the summer of 2021, precisely when the world was grappling with a severe chip shortage. This restricted my options significantly in terms of part selection. Luckily I bought some of the parts before I started the design so the chip shortage did not affect me as much.

For the PCB design, I turned to KiCad—a free and open-source PCB design software. The process of making ares started before I made the block diagram. Crafting an all-encompassing flight computer demands a wealth of experience for effective execution. To that end, there were two precursor designs before Ares.


![](/images/blog/ares-flight-computer/progress.PNG)

The lessons learned from the successes and pitfalls of these preliminary projects equipped me with the know-how to bring Ares to life. My approach to Ares began with conceptualizing a block diagram to illustrate component interconnectivity. Instead of committing to a singular PCB right away, I devised breakout boards to test individual components.

![](/images/blog/ares-flight-computer/breakout.jpeg)

This has the benefit of allowing me to evaluate each component's performance in isolation. Should any component falter, it afforded me the flexibility to tweak the design for the final iteration.

Part List:
- FCC: STM32F405RGT
- NAV: STM32L152C8T
- IMU: BMX160 (discontinued)
- Barometer: BMP388
- Accelerometer: LIS2HH12
- Flash Chip: W25Q32JVSS
- Radio: nRF24L01P


## What I learned

> The best way to learn is to do; the worst way to teach is to talk - Paul Halmos

The journey of crafting Ares was an enlightening one, although not devoid of challenges. For instance, what you see now as Ares is the third iteration of its design; the first two were prototypes that presented their own sets of challenges. A significant learning curve I faced was underestimating the  programming of the STM32 in C++. Yet, each challenge made me wiser, and I garnered a wealth of knowledge from efforts that I would have previously thought were beyond my grasp.

Personally, hands-on projects have always been the most effective method for me to acquire a new skill. My inspiration for this project was kindled by YouTube creators: Joe Barnard, who constructed a flight computer, and Phil from "Phil's Lab," who introduced me to the nuances of PCB design. Drawing inspiration from these talented creators and immersing myself in hands-on experimentation has not only honed my skills but also fueled my passion for rocketry and electronics.

<p align="center">
<iframe width="600" height="350"
src="https://www.youtube.com/embed/V7Fv0wA5XYY">
</iframe>
</p>