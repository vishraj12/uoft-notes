#### Synchronization Definition
- Processes and threads interact in a multi-programmed system 
	- To share resources
	- To coordinate execution 
- Arbitrary interleaving of thread executions can have unexpected consequences 
	- Need a way to restrict possible interleavings of executions 
	- Scheduling is invisible to the application
- Synchronization is the mechanism that gives us this control
#### Types of Synchronization
- Enforce single use of a shared resource 
	- Called critical section problem 
	- e.g. if Hello and Goodbye is printed to console -> HeGolloodbye shouldn't be the result
- Control order of thread execution
	- e.g. ensure output is always Hello before Goodbye
	- e.g. parent waits for child to finish
#### Reason for Synchronization 
- Two concurrent threads manipulate a shared resource without any synchronization 
	- Outcome depends on the order in which accesses take place -> race condition 
- Need to ensure that only one thread at a time can manipulate the shared resource -> we can reason about program behaviour -> need synchronization 
#### Critical Section Solution 
- Given: 
	- Set of n threads
	- Set of resources shared between the threads
	- Segment of code with accesses shared resources called critical section (CS)
- Want to ensure: 
	- Only one thread at a time can execute in the critical section 
	- All other threads are forced to wait on entry
	- When a thread leaves the CS, another can enter
- Requirements:
	- Mutual Exclusion: Only one thread can be executed in CS at a time -> ensures atomicity
	- Progress: If no thread are in their CS and some threads wish to enter their CS, then a thread must be selected to enter their CS within a limited amount of time -> no deadlock/livelock
	- Bounded Wait: Once a thread requests to enter its CS, and before access is granted, there exists a bound on the number of times other threads are allowed to enter their CS  -> no starvation
- Critical Section Problem: 
	- Design a protocol that threads can use to cooperate
		- Each thread must request permission to enter its CS, in its entry section 
		- CS is followed by an exit section 
		- All other code is the remainder section 
	- Each thread is executing at non-zero speed -> no assumptions about relative speed
#### Lock Data Type
- Use the lock data type to encapsulate critical section entry and exit
- A lock data type has internal state
	- Unlocked or locked
	- May keep track of the owner
	- May keep list of threads waiting to acquire the lock 
- 2 operations:
	- lock() -> Returns when the calling thread has acquired the lock, if no other thread holds the lock the calling thread will acquire the lock and return immediately. Otherwise, the calling thread will block until it can acquire the lock
	- unlock() -> Releases the lock; if no other threads are waiting to acquire the lock, one of them should be able to complete its lock() operation following an unlock()
#### Solutions to Implementing Locks
- Control Interrupts: 
	- Not safe to allow user-mode processes to use this mechanism
	- Doesn't ensure mutual exclusion on multiprocessors
		- On an uniprocessor, context switch can't happen during the critical section
		- On a multiprocessor, disabling interrupts does not prevent other threads from running on a different processor
	- Restricts what can be done inside the critical section
- Spinlocks: 
	- Wasting CPU cycles but at least mutual exclusion 
	- Problems: 
		- Spinlocks are built on machine instructions
		- Can be busy waiting
		- Starvation is possible
			- When a thread leaves its CS, the next one to enter depends on scheduling 
			- A waiting thread could be denied entry indefinitely
		- Deadlock is possible through priority inversion 
- Sleep Locks:
	- Instead of spinning, put thread into blocked state while waiting to acquire a lock 
	- Requires a queue for threads that are waiting for the lock 
	- But lock variable is shared -> To synchronize access to the list of threads, use a lower-level mechanism, like a spinlock or disabling interrupts
#### Summary
To ensure mutual exclusion
- Interrupt Disabling 
- Atomic Instructions
- Software (impractical)
How to wait for CS
- Spin
- Yield 
- Sleep