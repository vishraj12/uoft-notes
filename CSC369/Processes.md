#### Definition
- Process is a program in execution (a running program)
- Things to remember:
	- OS provides each process with a virtual execution env
	- Direct Execution Model
#### OS View of a Process
- A process contains all of the state for a program in execution:
	- An address space
		- The code and data for the executing program
		- An execution stack containing the state of procedure calls
	- The program counter (PC) indicating the next instruction
	- A set of general-purpose registers with current values
	- A set of operating system resources
		- Open files, network connections, signals etc
- A process is named using its process ID (PID)
- Process Control Block (PCB)
	- Data structure where the OS stores data about the process
	- Includes:
	- Process state (ready, running, blocked)
	- Program Counter (PC)
	- CPU Registers
	- CPU Scheduling Information
	- Memory Management Info
	- Accounting Info
	- I/O Status Information: open files
- OS maintains a collection of state queues that represent the state of all processes in the system. 
- Typically, the OS has one queue for each state.
	- Ready, waiting for event X, etc
- As a process changes state, its PCB is unlinked from one queue and linked into another
- Context switch -> switching the CPU to another process by:
	- Saving the state of the old process
	- Loading the saved state for the new process
	- Happens when:
		- Current process calls yield() system call (voluntary context switch)
		- Current process makes a system call and is blocked
		- Timer interrupt handler decides to switch processes
	- What goes into 'context':
		- State of a process at a particular point in time
		- Info needed to suspend a running process (and later be able to resume it)
		- Context data includes registers:
			- PC
			- SP
			- Status Registers (SR)
			- General Purpose Registers (GPREGS)
			- Page Table Base Register (PTBR)
#### Lifecycle of a Process
- New -> Ready -> Running
- Create new process
	- Create new PCB + user address space
- Load executable
	- Initialize start state for process
	- Change state to ready
		- Process becomes runnable for the first time
- Dispatch process
	- Change state to 'running'
- OS manages processes by keeping track of their state
	- Different events cause changes to a process state which the OS must record and implement
#### Context Switch 
- How it happens:
	1. A user process is currently running 
	2. Timer interrupt is triggered 
		- CPU will finish execution of current instruction first
	3. CPU will save PC, SP, SR and then update them in preparation for interrupt handling
	4. Interrupt handler will save other registers on the stack 
	5. Save context for current running process 
	6. Load context for next running process
	7. Replace GPREGS for user process B
	8. Last instruction of interrupt handler is iret 
		- Restores PC, SP, SR, clears interrupt request and resumes execution