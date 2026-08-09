# August 8 - Start

This is mainly going to serve as a practice exercise for creating 4-layer pcbs. As mentioned in the README, I'm following along with [this](https://www.youtube.com/watch?v=nkHFoxe0mrU) tutorial to create the base board (that's why this is marked as T4 on forge) but I will make some changes to the design as I see fit, and will try to do the majority of steps myself. This board is going to use the STM32WB55CEU6 MCU to allow for Bluetooth communication. I'm also going to be using KiCAD to actually create the schematic and design the PCB. With that out of the way, I'm going to jump straight into designing this. First, I imported the MCU symbol in KiCAD:

<img width="499" height="858" alt="image" src="https://github.com/user-attachments/assets/5ea7569d-5db5-4161-8914-9d6bab7efcf4" />

Then I also created a new project in STM32CubeMX, which is a software that allows for easier assignment of pins depending on your needs:

<img width="722" height="698" alt="image" src="https://github.com/user-attachments/assets/ed36b81b-59a3-4468-9c01-10ad0ef5432a" />

Back in KiCAD, I added all of the decoupling caps. These were mostly based off [this](https://www.st.com/resource/en/application_note/an5165-how-to-develop-rf-hardware-using-stm32wb-mcus-stmicroelectronics.pdf) application note. To keep the board relatively simple, I merged VDD, VDDA, and VDDUSB, as well as VBAT of course. The tutorial also didn't add a second 100pF to the VDDRF pin, but I added it just for good measure.

<img width="1294" height="432" alt="image" src="https://github.com/user-attachments/assets/e212b979-34b2-4997-a088-c5e6f76ff8b6" />

Next I implemented SMPS, this basically helps improve low power performance. I did this by tying the VLXSMPS and VFBSMPS pins together connected by two inductors and a capacitor as specified by the documentation:

<img width="789" height="387" alt="image" src="https://github.com/user-attachments/assets/bd28f49f-8b9c-4e34-b423-4319f3dc2508" />

Then, I added the crystal oscillators. This MCU is kind of weird in that it requires both a high-speed and low-speed crystal oscillator. The high-speed crystal actually doesn't require any load capacitors as well. For the low-speed crystal's load capacitors, I'm not going to actually add their values until I decide which crystal I'm going to be using.

<img width="551" height="651" alt="image" src="https://github.com/user-attachments/assets/5a7f3071-c87d-4895-8d33-37aa03def775" />

**Total Time Spent: 1 Hour**
