#### OS Relevance
- Almost all appliances have a small computer (SOC) to run programs BUT most of them run only one program at a time -> need for OS when multiple programs are run simultaneously
- Solves Multi-tasking 
- Without it, you need to do everything manually (like in Assembly) -> even if its one program as most programs usually depend on libraries
- Secondary Function: Abstraction 
	- Don't have to deal with hardware directly 
	- Can call OS with a more universal, intuitive and portable interface
#### Broad vs. Narrow OS
- Distributions -> Include kernel, compilers, libraries, system tools etc
	- Broad OS
	- iOS, Huawei, Ubuntu
- Kernels -> Pure kernel
	- Narrow OS
	- Linux, XNU
#### What OS does
##### Serve
- Provide interfaces to the hardware -> read/write
- Responds to requests (system calls)
- Helps process communications between each other (IPC)
##### Protect
- Isolation: ensures processes don't interfere with or know of, each other
- Protection: Prevents faulty processes from crashing the entire system
- Security: Protects the system from processes with malicious behaviours
##### Manage
- Manages all resources, including:
	- Hardware -> CPU, memory, disks
	- Virtual resources -> pipes, sockets, ports
- Allocates and reclaims resources to and from the processes
- Start, run, pause/resume, and destroy processes 
#### OS Objective
- Primary: Convenience for the user
	- Must be easier to compute with the OS than without it
- Secondary: Efficient operation of the computer system 
- Contradictory objectives but the precedence depends on the purpose of the computer system -> needs balance
- OS Themes
	- Virtualization -> presents physical resource in a more general, powerful or easy-to-use form + presents illusion of multiple resources
	- Concurrency -> coordinates multiple activities to ensure correctness
	- Persistence -> Some data needs to survive crashes and power failures
#### Virtualization (Goal of Serve, Protect, Manage)
- OS needs some sort of resource -> e.g. CPU, performance (aims to make this as low as possible)
- Each process thinks its the only one on the machine 
	- Lives in a private virtual environment
	- Can access everything in this virtual environment without worrying about others
- All hardware/other resources are virtualized (abstracted)
	- Processes only have access to virtual resources
		- Accessed through portable, well-defined, and easy-to-use OS interfaces 
			- e.g. system calls, files, sockets
	- OS translates process operations on virtual resources into actions on the physical hardware
- Everything is virtualized
	- All Hardware
		- CPU
		- Memory
		- Network
		- Storage
		- I/O Devices
	- All virtual resources
		- Mutex 
		- File Descriptors
		- Sockets 
		- Tunnels
- Goal: Provide isolation and enable multitasking
	- Isolation:
		- OS must prevent a process from doing anything it shouldn't
			- Accessing resources that do not belong to it
			- Interfering with other processes
			- Messing with the OS itself
		- Multitasking:
			- OS must have control over which process runs at all times
- To virtualize a CPU: 
	- OS goes through each instruction to see if each one is legal
		- Problem: Too time-consuming, inefficient
	- Direct Execution -> Next instruction is fetched from the process's code and let it run (no resources during execution) -> Process run on CPU not kernel
		-  Cons: 
			- Need to restrict what operations a process can perform (can't do this yet)
			- Need a way for the OS to regain control (can't do this yet either) -> infinite loop
		- Solution to the Problems Above: Limited direction execution
#### Main Hardware Support for OS
- Protection domains
	- OS is allowed to do things that user processes are not allowed to do
- Interrupts 
	- CPU immediately transfers control to well-defined OS function 
		- Using a register that holds pointer to interrupt descriptor table (IDT)
		- Switches to OS protection domain
- Timers
	- Generates periodic interrupts, supporting scheduling
- Memory Management Unit
	- Using a register that holds pointer to page table base pointer (PTBR)
	- Provides each process with an isolated view of its memory
#### Protection Domains
- Dual-mode operation: User mode and system mode
	- a.k.a supervisor mode, monitor mode or privileged mode
	- intel has 4 'rings' for protection (technically only need to have 2)
- Add a mode bit to hardware and designate some instructions as privileged
	- Attempts to execute privileged instruction from user mode causes protection fault
		- Trap to OS
	- Protects OS from access by user programs, and protects user programs from others
#### Interrupts
- A hardware signal that causes the CPU to jump to a pre-defined instruction called the interrupt handler
	- A mode switch to kernel mode simultaneously happens here
- Causes
	- Software 
		- Software Interrupt
		- Also known as exceptions
		- e.g. attempt to execute illegal instruction, access illegal memory address, division by 0
	- Hardware
		- Requesting device requires immediate attention
		- e.g. keyboard stroke detected, network packet arrived etc
- Before an Interrupt
	- A CPU works by continuously fetching the next instruction, decoding it, and then executing the instruction
		- Fetch-decode-execute-cycle (Increment the PC)
	- Interrupt Vectors -> Contains an array of function pointers to interrupt handlers (also known as trap table, interrupt vector table etc)
	- At boot time, the OS initializes the interrupt descriptor table (IDT) and then sets the IDTR to point to the table
- Interrupt Occurs
	- **CPU** will do the following things to running an interrupt handler:
		1. Finish executing the last instruction
		2. Discard everything else in the pipeline
		3. CPU changes mode, disable interrupts
		4. Interrupted PC value is saved
		5. IDTR + interrupt number is used to set PC to start of interrupt handler
		6. Execution continues
- Interrupt Mechanism supports virtualization by:
	- Any illegal instruction executed by a process causes a software-generated interrupt (aka an exception)
		- OS gets control whenever the process does something it shouldn't
			- Attempting to execute a privileged instruction
			- Illegal memory access
			- Illegal instructions
	- Periodic hardware-generated timer interrupt ensures OS gets control at regular intervals
		- Can switch processes to give virtual CPU illusion
#### Bootstrapping
- Hardware stores small program in non-volatile memory
	- BIOS - Basic Input Output System
		- Replaced by UEFI (irrelevant)
	- Knows how to access simple hardware devices
		- Disk, keyboard, display
- When power is first supplied, this program starts executing
- What it does:
	- Loads bootloader from boot sector of hard drive
	- Bootloader finds and loads OS code into memory
	- Starts running OS code
- Hardware starts in system mode, so OS code can execute immediately
- OS initialization:
	- Initialize internal data structures
		- Machine dependent operations are typically done first
	- Create first process (init)
	- Switch mode to user and start running first process
	- Wait for something to happen
		- Always an interrupt or exception
		- OS is driven by external events
			- External to OS (not the computer)