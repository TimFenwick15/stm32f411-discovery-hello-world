An STM32F411 Discovery (Arm Cortec M4) hello world :heart:

Several toolchains are available for Arm Cortex M based devices:
- Keil
- IAR
- GNU
- STM32CubeF4

We'll use GNU in Eclipse because a similar approach will work on Windows and Linux, and it is typically recommeded in Reddit threads.

This was setup by following https://www.youtube.com/watch?v=8p2phUTRebg

## Requirements
- I'm using a STM32F411E Discovery
- A USB A to USB B cable (not included with the board)
- Windows 10 (although most of these tools should be available for older Windows, and Linux)

## Steps
1. Download an Eclipse installer from https://www.eclipse.org/downloads/
2. Run the installer to install Eclipse
  - This requires Java JRE. This can be installed from https://www.java.com/en/download/
3. Open a Command Prompt, and run $ xpm install --global @gnu-mcu-eclipse/windows-build-tools
  - This requires NPM (part of NodeJS), which can be downloaded from https://nodejs.org/en/download/
  - When NPM is installed, npx can be installed by opening a Command Prompt and running $ npm i g xpm
4. In a Command Prompt, install OpenOCD with $ xpm install --global @xpack-dev-tools/openocd@latest 
5. Download the GNU Arm Embedded toolchain from https://launchpad.net/gcc-arm-embedded/+download
6. Run this installer
7. Download the ST-LinkV2 utility from https://www.st.com/en/development-tools/st-link-v2.html#. Get the part called "STSW-LINK004" in the "Tools & Software" tab
  - ST ask for a name and email address before letting you download
8. Run this installer
9. Run Eclipse. Optionally pick a location for your workspace
10. From the top menu, click Help > Install New Software
11. Click Add
12. Set the location as: http://gnu-mcu-eclipse.netlify.com/v4-neon-updates/
13. Pick the following items to install (all with a "GNU ARM C/C++" prefix):
  1. Cross Compiler
  2. Documentation (Placeholder)
  3. Generic Cortex-M Project Template
  4. J-Link Debugging
  5. OpenOCD Debugging
  6. Packs (Experimental)
  7. STM32Fx Project Templates
14. Click Next
  - I got a warning about installing unsigned software, but installed anyway :man_shrugging: :woman_shrugging:
  - Eclipse will ask to restart when the download is complete, allow it to
15. When Eclipse launches, click Workbench from the welcome screen
16. From the top menu, click File > New > Other. Click "C Project", and choose "STM32F4xx C/C++ Project"
17. Change the "Chip family:" to "STM32F411xE" 
  - I missed ths, and had ended up going to find a copy of stm32411xe.h on the internet, and changing my build defines to the correct board
  - Build defined variables are found by right-clicking the project > Properties > C/C++ General > Paths and Symbols. If you change one, check "Add to all configurations" and "Add to all languages" to make sure they match
18. Change "Use system calls" to "POSIX"
  - Why? :mag:
19. Accept the rest of the defaults
20. Build the project. There will be a compiler warning from _initialize_hardware.c asking you to check it against your board configuration. Check that a build variable matching the board name is defined. In this case: STM32F411xE (if it is defined, it will activate the code blocks wrapped in "#if defined(STM32F411xE) ... #endif" tags, unactivated code is greyed out)
21. Click Debug > Manage Configurations. Right click "GDB OpenOCD Debugging", choose New. Under "Common" tab, choose "Shared file", under "Debugger" tab, enter into config options: -f board/stm32f4discovery.cfg
22. Connect the STM32 board to your computer with a USB A to USB B cable. If it has not been programmed before, the default application blinks 4 LEDs, and pressing the button B1 activates the MEMS sensor mode
23. Click the run button to flash the LED blink project to the board. The LD4 LED will blink, and output will be written to the Eclipse console

