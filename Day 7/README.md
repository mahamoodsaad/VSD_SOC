* SoC (System on Chip) is an integrated circuit that combines all components of a computer or electronic system—such as CPU, memory, I/O, and peripherals—onto a single chip.

* The RVMYTH core is a basic RISC-V CPU developed for learning and lightweight applications, offering a hands-on example of a RISC-V processor implementation.

* PLL (Phase-Locked Loop) is an electronic control system that synchronizes an output signal’s phase and frequency with a reference signal, commonly used in clock generation, frequency synthesis, and communication systems.

* DAC (Digital-to-Analog Converter) is an electronic device that converts digital signals (typically binary values) into corresponding analog voltages or currents, enabling digital systems to interface with analog components.

**Setting up the Project Directory and cloning the project:**

<img width="744" height="538" alt="Screenshot 2025-07-02 at 11 21 08 AM" src="https://github.com/user-attachments/assets/bcf8db76-b825-4bc1-a17a-30ff0e1463ab" />

**TLV to Verilog Conversion:**

<img width="750" height="534" alt="Screenshot 2025-07-02 at 11 31 57 AM" src="https://github.com/user-attachments/assets/bb1a866c-56c5-4d46-9ca0-510888e5da00" />

**Pre synthesis simulation and viewing waveforms in GTKwave:**

<img width="750" height="533" alt="Screenshot 2025-07-02 at 11 43 06 AM" src="https://github.com/user-attachments/assets/36acfe62-cf7f-4dcc-9147-cc72e7689df6" />

<img width="749" height="533" alt="Screenshot 2025-07-02 at 11 43 30 AM" src="https://github.com/user-attachments/assets/3e7aeb14-9141-4c68-b2f3-489623bc432d" />

The following signals are displayed:
* CLK: This is the input clock signal for the RVMYTH core.
* reset: This is the reset input to the RVMYTH core.
* OUT: This is the output signal from the VSDBabySoC module.
* RV\_TO\_DAC\[9:0]: This is a 10-bit output port from the RVMYTH core.
* OUT (real type): This is a wire with a real datatype capable of representing analog values.

To view the signal as analog, set its Data Format to Analog → Step.
