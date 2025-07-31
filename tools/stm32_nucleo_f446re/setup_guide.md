# Getting Started with STM32 Bare-Metal Development

This guide outlines the essential software and hardware components you'll need to set up your development environment for bare-metal programming on STM32 microcontrollers. We'll focus on using free and open-source tools, which provide excellent control and insight into the embedded development process.

## Prerequisites

Before you begin, ensure you have:

* A basic understanding of the C programming language.

* Familiarity with using the command line (Terminal on Linux/macOS, Command Prompt/PowerShell on Windows).

* A general understanding of what microcontrollers are and how they interact with hardware.

## 1. Core Toolchain (GNU Arm Embedded Toolchain)

This is the heart of your embedded development setup. It provides the compiler, assembler, linker, and other utilities necessary to turn your C code into executable binaries for ARM Cortex-M microcontrollers.

* **Component:** `arm-none-eabi-gcc`

* **Purpose:** Compiles your C/Assembly code into machine code for ARM Cortex-M microcontrollers. `none-eabi` indicates that it's a "bare-metal" toolchain (no operating system, no standard C library for embedded).

* **Installation (Linux Specific):**

    ### Using your Distribution's Package Manager (Recommended for Simplicity)

    This is often the easiest way, but the version available might not always be the very latest.

    1.  **Update package lists:**
        ```bash
        sudo apt update
        ```
    2.  **Install the toolchain:**
        ```bash
        sudo apt install gcc-arm-none-eabi
        ```
        *(Note: If you're using a different Linux distribution, replace `apt` with your package manager, e.g., `dnf install arm-none-eabi-gcc` for Fedora, `pacman -S arm-none-eabi-gcc` for Arch Linux, etc.)*
    3.  **Verification:** Open a new terminal and run:
        ```bash
        arm-none-eabi-gcc --version
        ```
        You should see the version information printed.


## 2. Flashing and Debugging Utility (OpenOCD)

OpenOCD (Open On-Chip Debugger) is a free and open-source tool that provides a bridge between your computer and the microcontroller's debug interface (like SWD or JTAG) via a hardware debugger (e.g., ST-Link, J-Link). It's used for flashing compiled code onto the chip and for debugging.

* **Component:** `OpenOCD`

* **Purpose:** Communicates with your hardware debugger to flash firmware onto the STM32 and enables GDB for debugging.

* **Installation (Linux):**

    * **Using your Distribution's Package Manager (Recommended):**
        ```bash
        sudo apt update
        sudo apt install openocd
        ```
        *(Again, replace `apt` with your package manager if different.)*

    * **Verification:** Open a new terminal and run:
        ```bash
        openocd --version
        ```
        You should see the version information.

## 3. Debugger (GDB)

GDB (GNU Debugger) is a powerful command-line debugger that comes bundled with the GNU Arm Embedded Toolchain. It works in conjunction with OpenOCD to allow you to step through your code, inspect variables, set breakpoints, and examine memory on the target microcontroller.

* **Component:** `arm-none-eabi-gdb`

* **Purpose:** Provides debugging capabilities for your embedded applications.

* **Installation:** This is installed automatically when you install the GNU Arm Embedded Toolchain (Step 1).

* **Verification:** Open a new terminal and run:
    ```bash
    arm-none-eabi-gdb --version
    ```
    You should see the version information.

## 4. Development Environment (IDE / Code Editor)

While you can use any text editor, a good IDE or code editor with relevant extensions can significantly enhance your development workflow.

* **Option 1 (Recommended for Flexibility): Visual Studio Code (VS Code)**

    * **Installation:** Download and install VS Code from the [official website](https://code.visualstudio.com/).

    * **Essential Extensions:**

        * **C/C++ Extension Pack (by Microsoft):** Provides IntelliSense, debugging, code formatting, etc.

        * **Cortex-Debug (by Marus):** Crucial for integrating GDB and OpenOCD for seamless debugging of ARM Cortex-M projects directly within VS Code.

        * **Makefile Tools (by Microsoft):** Helps with navigating and running Makefile targets.

        * **Better C++ Syntax (by cheshirekow):** Improves syntax highlighting.

* **Option 2 (Integrated Environment): STM32CubeIDE**

    * **Installation:** Download and install STM32CubeIDE from the [STMicroelectronics website](https://www.st.com/en/development-tools/stm32cubeide.html). You'll need an ST account.

    * **Purpose:** This is ST's official IDE, based on Eclipse. It includes the GNU Arm Embedded Toolchain and OpenOCD/ST-Link tools. While it primarily generates projects using ST's HAL/LL libraries, you can configure it for bare-metal development or use it as a convenient way to get all tools in one package. It also has excellent device configuration tools.

## 5. Hardware Debugger & Drivers

You'll need a physical hardware debugger to connect your computer to the STM32 board.

* **Hardware:**

    * **ST-Link/V2 or ST-Link/V3:** Many STM32 Nucleo and Discovery boards have an integrated ST-Link. You can also buy standalone ST-Link V2 clones cheaply. This is the most common and recommended debugger for STM32.

    * **J-Link:** A more powerful and versatile debugger, but usually more expensive.

* **Drivers (Linux):**

    * For ST-Link, you typically need to set up `udev` rules to allow your user to access the device without `sudo`. These rules are often installed automatically with OpenOCD or can be found in OpenOCD's documentation (look for `60-openocd.rules` or similar). A common location is `/etc/udev/rules.d/`.
    * For J-Link, drivers are usually part of the J-Link Software and Documentation Pack from Segger's website.

## 6. Serial Terminal (Optional but Highly Recommended)

For debugging and printing messages from your microcontroller (e.g., via UART), a serial terminal program is indispensable.

* **Linux:**

    * `minicom` (command-line: `sudo apt install minicom`)

    * `screen` (command-line: `sudo apt install screen`)

    * `picocom` (command-line: `sudo apt install picocom`)



## Next Steps

Once you have these tools installed, you're ready to start writing and flashing your first bare-metal STM32 programs! You'll typically:

1.  Write your C code (e.g., `main.c`).

2.  Create a `Makefile` to compile your code using `arm-none-eabi-gcc`, link it with the appropriate linker script, and convert it to a flashable format (e.g., `.hex` or `.bin`).

3.  Use `OpenOCD` (often called from your Makefile) to flash the `.hex` or `.bin` file to your STM32 board.

4.  Optionally, use `OpenOCD` and `arm-none-eabi-gdb` (or VS Code's Cortex-Debug extension) for interactive debugging.

Happy coding!