# RISCV-32I-AHD
Advanced Hardware Design Project



ALU
1. Design (alu.v)
- We designed an ALU which performs all the operations mentioned in the supplementary material. The ALU has 2 32 bit inputs, a 4 bit alu_select input and a 32 bit output
that stores the result of the operarion. The alu_select opcodes have been defined in a sepearte header file that hold all the opcodes for different components of the 
processor. For, example ADD instruction has the word ADD at the alu_select case instead of the actual binary number. This makes the code very legible and easy to 
understand. 

2. Testbench (alu_tb.v, alu_test.txt)
- The testbench for the ALU is pretty straightforward. It has registers that store the inputs and wire that holds the output. I chose to read a alu_test.txt file that
will consist of all my testcases. This way instead of writing each and every testcases, there will be a single file which will hold the testcases. I also added 2 counters, a pass count and a fail count. These counters will keep track of all the testcases whether they have passed or failed. 


Program Counter
1. Design (program_counter.v)
- Program Counter is the register that stores and sends out address of instruction to execute. It sends out 32 bit address, however last two LSBs are tied to 0, so that only starting address of instruction word is generated. We have also implemented some additional functionality in the PC module itself, first, increment by 4 and second, branch/jump to an immediate value. The output is decided between the two by input imm, generated by control unit. A failsafe mechanism is built in to reset PC to 0x01000000 in case it reaches an out-of-bounds address not supported by IM.

2. Testbench (program_counter_tb.v, program_counter.csv)
- We are using file IO to drive inputs and compare outputs with a csv file containing all the testcases. The testcases cover the following scenarios:
	- Tests regular counter operation (increment by 4)
	- Tests immediate operation (update value as per immediate input)
	- Tests return to regular counter post immediate address update
	- Tests if out-of-bounds address leads to reset condition
		- If immediate address is out of bounds
		- If PC counts to last address in IM
	- Test external reset functionality


Instruction Memory
1. Design (instruction_mem.v, imem.mem)
- Instruction memory has been implemented as a 2Kbyte instruction memory designed to store 512x4-byte long instruction words. The memory addresses start from 0x01000000 and goes to 0x010007FF with one byte of data stored at each address. Hence, one instruction word spans 4 addresses in little endian format. Only a complete instruction word can be read from the memory. 

2. Testbench (instruction_mem_tb.v, imem_testcases.csv)
- Design loads imem.mem file to memory. Testcases are written to randomly access all addresses and stored instruction value is compared. Hence all addresses in memory are tested.


Register File
1. Design (register_file.v)
- Register file is a collection of 32 32-bit wide registers, where R0 is hardwired to 0. Reset signal is present to clear registers R1 to R31. Read and write functionality present (except for R0, which is read-only).

2. Testbench (register_file_tb.v, register_file.csv)
- Testcases are stored in a csv file and cover the following scenarios:
	- Tests register write operation when we signal is high
	- Tests if junk write is prevented when we signal is low
	- Tests rs1 and rs2 read
	- Tests read on R0 (should be 0 even without reset, hence first testcase)
	- Tests if write on R0 is prevented even if we signal is high
  - Tests reset functionality and read/write post reset
