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

# August 14 - RF Hardware and USB-C

Next up is the RF hardware. This is pretty new to me so I'm going to rely pretty heavily on the tutorial. STM's official application notes (AN5165) say to setup the RF hardware like this:

<img width="1092" height="360" alt="image" src="https://github.com/user-attachments/assets/fa55a09f-4df9-46d4-a6da-0a8dc52912e4" />

Using that as a reference, I first setup the matching network. This basically uses pi-network impedance matching to keep the impedance equal on both sides, preventing signal reflection.

<img width="401" height="434" alt="image" src="https://github.com/user-attachments/assets/6788a9b1-da3b-4442-87cc-e79048233942" />

Next, I added a low pass filter. The specific model that the application note suggests is DLF162500LT-5028A1, which didn't have a schematic and footprint available, meaning I had to make my own:

<img width="359" height="314" alt="image" src="https://github.com/user-attachments/assets/93c18ef5-f478-4491-84a6-3016f75c449d" />
<img width="797" height="475" alt="image" src="https://github.com/user-attachments/assets/3d3e5329-d946-483f-83ba-e22d91151408" />

Then I added a coaxial connector symbol for the UFL connector.
 
<img width="358" height="300" alt="image" src="https://github.com/user-attachments/assets/04421b2b-a87d-4000-9cca-ecf0f92e6897" />

Next I broke out the SWD pins for debugging as well as the reset pin in STM32CubeMX:

<img width="1028" height="1041" alt="image" src="https://github.com/user-attachments/assets/0bc9e309-c6f3-4cd6-86b5-65fb2723d850" />

Then I added it into the schematic. I'm using a 6 pin connector while the video uses something called a tag-connect, but tag-connect wires are expensive so I'm just going to stick with a simple connector.

<img width="570" height="721" alt="image" src="https://github.com/user-attachments/assets/7fa0f311-69f1-475f-905c-ae6046860da1" />

Added boot:

<img width="519" height="384" alt="image" src="https://github.com/user-attachments/assets/feaa4e29-ea03-4a2c-939f-9ffb1e854e43" />

Next I added USB-C, this is pretty standard but I also added an ESD protector, which is not something I've actually done in the past:

<img width="698" height="718" alt="image" src="https://github.com/user-attachments/assets/f143c28c-a458-4afd-868e-d9ce03329f76" />

To find a good LDO voltage regulator, I used STM32CubeMX to find the current requirements for the MCU.

<img width="343" height="26" alt="image" src="https://github.com/user-attachments/assets/1a84c387-745d-43e1-8f38-6b65aa1da9f3" />

Since it is so low, I can get away with using an LDO voltage regulator. The specific one I'm using is the MIC5365-3.3YD5. I'm using 4.7uF caps to shorten the BOM.

<img width="535" height="318" alt="image" src="https://github.com/user-attachments/assets/8d33a1dd-dbbb-4a20-be26-3d3cc77b8c92" />

**Total Time Spent: 1.97 Hours**

# August 23 - UART, LED indicator

Next I'm going to add some peripherals, specifically a UART header. This is pretty standard, I'm not going to go too overkill here. I'm using a socket because it's cleaner than pins:

<img width="327" height="254" alt="image" src="https://github.com/user-attachments/assets/6f81be49-e0c1-4cd3-853e-7d30ae07d1b1" />
<img width="800" height="737" alt="image" src="https://github.com/user-attachments/assets/3232c694-2003-4e47-ac63-dfb382758291" />

I'm also going to add an LED power indicator. Normally I just hook this up to power, but the tutorial actually uses a timer channel to control it through PWM, which is something new to me. The tutorial uses the PA7 pin, I'm going to use PB9 instead because my schematic is kind of cluttered in the PA7 area:

<img width="254" height="112" alt="image" src="https://github.com/user-attachments/assets/15ee6cec-81eb-48e5-b798-38b2b51c6d83" />

For the LED, in order for it work properly I need around 220R, so I'm just going to change all the other 100R resistors to 220R as well to simplify the BOM.

<img width="518" height="343" alt="image" src="https://github.com/user-attachments/assets/d7c65505-f622-4f54-bd47-1f4c6a8318d4" />

The schematic itself is now complete, I just need to clean some stuff up. I added some labels and a title block, as well as fixing KiCAD's automatic annotations. Additionally I assigned 12.5p load caps to the LSE crystal (later changed to 13p as LCSC doesn't have 12.5p)

<img width="1562" height="1083" alt="image" src="https://github.com/user-attachments/assets/3cee5d4a-69fc-4607-a442-660416815821" />

I then went ahead and assigned footprints to all of my symbols. I also just removed the series resistors for the crystals. One thing I did decide on doing was to change the LDO to a cheaper version (TI TLV70033DDCR). I also updated my symbol fields to include the links to all the parts that I will be using to make ordering them easier (this took up the majority of the time).

<img width="599" height="832" alt="image" src="https://github.com/user-attachments/assets/63e783b4-734c-462b-9f07-81d613a3c355" />

I also added some net classes, and that basically finishes the entire schematic.

**Total Time Spent: 2.833 Hours**

# August 24 - Starting the PCB

I'm finally going to start actually designing the PCB. First, I need to do some basic setup. Obviously, this is going to be a 4-layer board (that's literally the reason I'm doing this), and since I'm using RF hardware I also need to make it impedance controlled. I did this by checking the box for it in KiCAD. I also changed the layer thickness and material according to JLCPCB's specs:

<img width="793" height="474" alt="image" src="https://github.com/user-attachments/assets/a7d2d4e5-e665-4f11-82e3-490fd05e5237" />

I also updated the design rule constraints based off of what JLCPCB allows:

<img width="379" height="587" alt="image" src="https://github.com/user-attachments/assets/f24392fa-1653-424b-98a1-bd17f09991da" />

I spent a lot of time after trying to figure out impedance, but it's late so I'll do that later.

**Total Time Spent: 0.66 Hours**

# August 25 - Impedance Research + PCB

I did some [research](https://www.youtube.com/watch?v=0fteCxn5XXA) on impedance so I can find out how to actually calculate the right impedance trace width for this board. From that video, I calculated that I'm going to need a trace width of 0.3586mm. I did this by using [JLCPCB's impedance calculator](https://jlcpcb.com/pcb-impedance-calculator), which greatly simplifies the process of calculating impedance as they have all the dielectric constant of their different board stack-ups built-in. For some reason, the main stack-up type they use is the last option on the calculator, which kind of confused me at first. I also calculated the impedance of the USB differential pair, which, with a target resistance of 90R and spacing of 0.15mm, turned out to be 0.2474mm. I then proceeded to add these as predefined track sizes, along with all the other predefined values that were added from the video, before adding all of my components to the PCB editor:

<img width="782" height="189" alt="image" src="https://github.com/user-attachments/assets/73be1a11-5d5c-4d65-a010-832eab15214c" />
<img width="537" height="566" alt="image" src="https://github.com/user-attachments/assets/cca86276-897b-4ba0-aec7-909ed7f29faf" />

First thing I did was go into my schematic and add some netlabels in places where I had forgotten them, pretty straightforward. I also didn't have a lot of the 3D models for the components so I went ahead and added all of the ones I didn't have. For the USB-C model, the specific one that I'm going to use doesn't have a STEP model available, so I had to use one of KiCAD's built in ones that didn't quite match but is good enough for this purpose for me to not care.

<img width="684" height="447" alt="image" src="https://github.com/user-attachments/assets/f7847cdd-c02d-4ad3-9d07-db13ba01a64c" />

**Total Time Spent: 0.817 Hours**

# Continued PCB

The first thing I did was add colors to my net classes to help me during laying out/doing traces on the PCB.

<img width="1104" height="954" alt="image" src="https://github.com/user-attachments/assets/73ba3df0-7000-4d12-a00a-c986964975cc" />

For the actual layout of the PCB, I'm trying a different approach that was used in the video, and that's to first create a very rough layout of the major components and build from there. Before what I did was try to spread out from the MCU to the rest of the board, but this approach feels like I'm planning the entire layout of the board better. The board is going be very long compared to its width, with the USB connector on one side and the RF antenna on the other. I'm kind of just placing stuff both where the video says to place it and where I think it would fit the best.

I originally had the SWD socket lined up vertically like how the video did it with tag connects:

<img width="953" height="478" alt="image" src="https://github.com/user-attachments/assets/ef2e856d-850f-442a-8780-3540458da25a" />

However, after adding the ESD protection on the USB port, I realized that drawing the traces around the socket wouldn't be optimal, so I just moved it down horizontally aligned with the UART socket:

<img width="1122" height="823" alt="image" src="https://github.com/user-attachments/assets/5cdbe729-480b-466c-a6e2-bf99026965d6" />

A good workflow that I'm finding effective is to do the thing where you layout all the major components first, but I'm also cleaning up component placements in relation to each other while I'm creating the initial layout. (RF):

<img width="678" height="780" alt="image" src="https://github.com/user-attachments/assets/0544e429-e2db-4e2f-88f7-b555653e719e" />

Load caps for the LSE crystal. I might change the orientation of the GND pins on the resistors later:

<img width="833" height="411" alt="image" src="https://github.com/user-attachments/assets/c96fb2a4-e146-4756-bafd-9b05f475b9b2" />

I also realized that I had gotten a couple of caps (C13, C15) wrong in the BOM while I was laying out the components for SMPS:

<img width="613" height="486" alt="image" src="https://github.com/user-attachments/assets/9bb58b80-7b42-42b7-8d8d-6e521f5534c1" />

That's all the important components done, I'll move on to decoupling caps in my next session.

<img width="1089" height="531" alt="image" src="https://github.com/user-attachments/assets/3db33d6a-7742-42f2-873e-93337ec0ff3f" />

**Total Time Spent: 1.7 Hours**
