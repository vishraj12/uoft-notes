#### OS Relevance
- Solves Multi-tasking 
- Without it, you need to do everything manually (like in Assembly)
- Secondary Function: Abstraction 
	- Don't have to deal with hardware directly 
	- Can call OS with a more universal, intuitive system
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
- Convenience 
- Efficient operation of the computer system 
- Contradictory objectives but the precedence depends on the purpose of the computer system -> needs balance
#### Virtualization
- Each process thinks its the only one on the machine 
	- Lives in a private space
	- Can access everything in this virtual environment without worrying about others
- All hardware/other resources are virtualized (abstracted)
- Processes only have access to virtual resources
- Virtualize CPU -> Prevent a process from accessing/interfering things it shouldn't or messing with the OS itself
- 