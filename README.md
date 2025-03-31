# CIS-310-Assignment-2-Implementing-a-Register-File-and-ALU
Register File and ALU Implementation Report
https://github.com/luluchen025/CIS-310-Assignment-2-Implementing-a-Register-File-and-ALU


1. Register File Design
The register file contains four 4-bit registers: R0, R1, R2, R3. It supports dual-read and single-write operations with the following components:

Write Mechanism
A 2-bit selector (Sel_write) chooses the destination register
A 4-bit input (Write_data) is passed through a MUX to route data to the appropriate register.
The CTRL-registers 8-bit line is split into 4 control chunks (2 bits per register)
Registers are written on the rising clock edge (clk) if their control signal is active

Read Mechanism
Two 2-bit selectors (Sel_read1 and Sel_read2) control connect to separate 4-to-1multiplexers that allow simultaneous reads from any two registers
Outputs are labeled Read_data1 and Read_data2

2. ALU Implementation
The ALU performs operations depending on control bits S_1, S_0, C_in. It includes the following components:

Inputs:
4-bit operands A and B
Control bits S_1, S_0, and C_in

Design Details
A multiplexer selects between B, bitwise NOT B, or constant.
A 4-bit adder processes A and the selected B variant plus C_in
Outputs a 4-bit result C and an overflow flag

Supported Operations
S1
S0
Cin
Input to ALU
Output (D)
Micro Operation
0
0
0
B
A + B
Add
0
0
1
B
A + B + 1
Add with Carry
0
1
0
B̅
A + B̅ + 1
Subtract with Borrow
0
1
1
B̅
A + B̅
Subtract
1
0
0
(N/A)
A
Transfer A
1
0
1
(N/A)
A + 1
Increment A
1
1
0
(N/A)
A - 1
Decrement A
1
1
1
(N/A)
A
Transfer A



3. Testing

Register File Tests:
Drive Read Register 1 and Read Register 2 with test values (00, 01, 10, 11). 
Enable writes to each register and verify that the correct data appears on Read Data 1 and Read Data 2.

Test 1: Write a value to R0 and read it
Set Write_data = 0101
Set Sel_write = 00 (for R0).
Set CTRL_registers = 01 (enable write to R0).
Pulse clk.
Set Sel_read1 = 00 → Read_data1 should show 0101.


Test 2: Write to R1 and read it
Set Write_data = 1010.
Set Sel_write = 01 (for R1).
Set CTRL_registers = 0100 (enable R1 write)
Pulse clk.
Set Sel_read1 = 00 → Read_data1 = 0101 (confirm R0 still has old value).
Set Sel_read2 = 01 → Read_data2 = 1010 


Test 3: Write to R2 and read it
Set Write_data = 0011.
Set Sel_write = 10 (for R2).
Set CTRL_registers = 010000 (enable R2 write)
Pulse clk.
Set Sel_read1 = 10 → Read_data1 = 0011 
Set Sel_read2 = 01 → Read_data2 = 1010 (Remains)



Test 4: Write to R3 and read it
Set Write_data = 1000.
Set Sel_write = 11 (for R3).
Set CTRL_registers = 01000000 (enable R2 write)
Pulse clk.
Set Sel_read1 = 10 → Read_data1 = 0011 (Remains)
Set Sel_read2 = 11 → Read_data2 = 1000



ALU Tests:
Apply test vectors to A and B and vary (S_1, S_0, C_in) to check each operation:
Add (A + B) 
Add with Carry (A + B + 1) 
Subtract (A - B) and Subtract with Borrow (if needed) 
Transfer A 
Increment A 
Decrement A

1. Add (A+B)
S1
S0
Cin
Input to ALU
Output (D)
Micro Operation
0
0
0
B
A + B
Add



2. Add with Carry (A + B + 1) 

S1
S0
Cin
Input to ALU
Output (D)
Micro Operation
0
0
1
B
A + B + 1
Add with Carry




3. Subtract (A - B) and Subtract with Borrow (if needed) 

S1
S0
Cin
Input to ALU
Output (D)
Micro Operation
0
1
0
B̅
A + B̅ + 1
Subtract with Borrow
0
1
1
B̅
A + B̅
Subtract





4. Transfer A 

S1
S0
Cin
Input to ALU
Output (D)
Micro Operation
1
0
0
(N/A)
A
Transfer A
1
1
1
(N/A)
A
Transfer A




5. Increment A 

S1
S0
Cin
Input to ALU
Output (D)
Micro Operation
1
0
1
(N/A)
A + 1
Increment A



6. Decrement A

S1
S0
Cin
Input to ALU
Output (D)
Micro Operation
1
1
0
(N/A)
A - 1
Decrement A


