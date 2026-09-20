# bt-stm32
An STM32 board with Bluetooth capabilities, made primarily for the sake of practice. I'm making this mainly so I can improve my PCB design skill all around, but mainly to practice making 4-layer boards, as well as to practice SMD soldering. I'm following along with [this](https://www.youtube.com/watch?v=nkHFoxe0mrU) tutorial (with my own changes, of course) to make this board using KiCAD. Firmware wise, I'm currently only using a basic script to flash the onboard LED on and off, but I will change this as I see fit, and all revisions will be updated to the repo.

<img width="1171" height="837" alt="image" src="https://github.com/user-attachments/assets/8ce271ad-b2d3-489b-9b22-c5ed40b89b6c" />
<img width="1614" height="981" alt="image" src="https://github.com/user-attachments/assets/b946e142-2460-461c-9407-5eea06756643" />
<img width="765" height="436" alt="image" src="https://github.com/user-attachments/assets/8a9a852d-b6aa-4b67-acbe-c87a09a17d71" />

If you want to make this for whatever reason, I would HIGHLY recommend using an SMD stencil instead of JLCPCB's PCBA, the gerbers and the BOM are in the [production](/production) folder. You can also find the firmware, which is currently just a basic LED blink, in the [firmware](/firmware) folder, and the PCB files themselves in the [pcb](/pcb) folder.

Some general cost estimates (USD):
- PCB: $7.10
- Stencil: $7.22
- LCSC Components: $31.01

If you want to use the RF capabilities, you will need an external antenna. Just search up female U.FL antenna and you will be able to find a lot of antennas that will work perfectly fine.

*Submitted to Forge (Hackclub)*
*Credit to Phil's Lab and Claude*
