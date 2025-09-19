#### What is a function call?
- Review from CSC209
#### Definition
- A system call is a function call that invokes OS code
	- Whenever an application wants to use a resource that the OS manages, it asks for permission
	- How do we keep applications from just using a resource without asking permission?
- Invoking OS code:
	- Software Interrupts signal errors
		- Exception/Trap
		- If a process attempts to use a resource without permission or tries to execute illegal instruction
	- Hardware interrupts signal CPU that a hardware device needs attention
		- e.g. keyboard input, disk I/O completed
	- Software Interrupts can be used to request OS intervention
		- sys call
		- Some processors have a dedicated instruction for sys calls
			- MIPs syscall
			- Intel INT 0x80 (old), SYSENTER (new)
#### System Call Interface
- User program calls C library func with args
- C library func prepares args to be passed to OS
	- Has a numeric sys call number
- Executes special instruction enter to system mode
	- Transfers control to an interrupt handler for all sys calls
- Interrupt handler locates syscall handler with the syscall number
- Syscall handler routine runs and services the sys call request 
- Kernel must verify the arguments it has passed (for security purposes and to prevent kernel malfunction -> causes Blue Screen of Death)
- A fixed number of arguments can be passed in registers 
	- Often pass the address of a user buffer containing data
	- Kernel must copy data from user space into its own buffers
- Result is returned in a register:
	- %rax on Intel x86_64
- Tracing sys calls:
	- Powerful way to trace system call execution for an application 
	- Use the strace command
		- Can see all system calls made by a process
		- Including their args
	- ptrace() sys call is used to implement strace -> also used by gdb 
	- Library calls can be traced using ltrace()
- Sys calls parameters:
	- First parameter is always the syscall number
		- Stored in %rax
	- Can pass upto 6 parameters: 
		- %rbx, %rcx, %rsi, %rdi, %rbp, %rdx
	- If more than 6 parameters are needed -> use a pointer
	- Have to validate user pointers somehow (PROBLEM)
#### Sys Calls in Linux
- Can invoke any syscall from userspace using syscall 
	- Compiler is unable to type-check system call arguments
- C has library wrapper functions to simplify sys calls and make their invocation more robust

#### Why are sys calls the way they are?
- Safety issue -> level of abstraction instead of direct access
#### Machine Dependent vs. Machine Independent Code
Machine-Dependent Code:
- Bootstrap
- System Initialization
- Interrupt and exception
- I/O device driver
- Memory management
Machine-Independent Code:
- sys calls