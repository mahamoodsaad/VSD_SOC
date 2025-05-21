## Summary:
The chip design flow begins with chip modeling, where a software application is developed in the C programming language and compiled to generate an initial output o0. This application is then run on a modeled processor architecture to verify the correctness of the instruction set implementation, producing output o1. Ensuring that o0 equals o1 serves as a checkpoint for freezing the design specifications.
Next is the RTL architecture stage, where the hardware is described using an HDL like Verilog. The same application is simulated on the RTL version of the processor, generating output o2. o2 should match o1. The synthesizable RTL is used to build digital blocks, while analog components like PLLs and ADCs are designed at the transistor level. These IPs are integrated into a complete SoC using buses and GPIOs. The application is run again to produce output o3, which must match the previous outputs to ensure correct system-level behavior.
The RTL to GDSII step involves backend processes such as floorplanning, PNR, clock tree synthesis. This step culminates in the generation of the GDSII file, the final layout sent for fabrication. In the physical verification and fabrication phase, the layout undergoes DRC and LVS checks. Once verified, the chip is manufactured referred to as tape out and enters the tape n phase where it is returned in physical form for testing.
The post-silicon validation stage involves placing the chip on an evaluation board and testing it with peripheral inputs to generate output o4. Confirming o4 matches all prior outputs ensures that the chip’s functionality has been preserved through every phase. Finally, in the system integration step, the validated chip is embedded into end-user products such as iwatches, arduino etc.

## Tool Installation Screenshots:
<img width="985" alt="Screenshot 2025-05-13 at 11 38 37 PM" src="https://github.com/user-attachments/assets/d4cdfa5d-de23-4db6-948d-43fab165f2a7" />
<img width="985" alt="Screenshot 2025-05-13 at 11 47 31 PM" src="https://github.com/user-attachments/assets/76fafca8-e507-4d35-9be9-f2051dc64abd" />
<img width="985" alt="Screenshot 2025-05-14 at 12 19 04 AM" src="https://github.com/user-attachments/assets/38bc7d96-3dee-46fd-8bfc-c492f52d150e" />
<img width="985" alt="Screenshot 2025-05-14 at 1 43 31 PM" src="https://github.com/user-attachments/assets/cb1407e6-a99a-4cd7-9480-71232a5141f5" />
<img width="985" alt="Screenshot 2025-05-14 at 2 24 57 PM" src="https://github.com/user-attachments/assets/896c6d1d-b843-48be-af13-7329d410f3ce" />
<img width="985" alt="Screenshot 2025-05-14 at 2 33 37 PM" src="https://github.com/user-attachments/assets/d7485354-3fe9-434a-a716-5464c6703841" />
<img width="985" alt="Screenshot 2025-05-14 at 2 34 09 PM" src="https://github.com/user-attachments/assets/2098bcff-83e2-425a-92b1-6738aa408711" />

