# Operating Systems and Concurrency
### Process
Each process **provides the resources** needed to **execute a program**. A process has:
- \>= 1 thread of execution.
- a unique process identifier
- a priority class
- virtual address space
- executable code
- open handles to system objects
- a security context
- environment variables
- minimum and maximum working set sizes

Each process is started with a single thread, often called the primary thread, but can create additional threads from any of its threads.

### Thread
A thread is an **entity within a process** that can be scheduled for execution. All threads of a **process SHARE**:
- its virtual address space
- system resources.

In addition, each thread maintains:
- exception handlers
- a scheduling priority
- thread local storage
- a unique thread identifier
- a set of structures the system will use to save the thread context until it is scheduled. The thread context includes:
  - the thread's set of machine registers
  - the kernel stack
  - a thread environment block
  - a user stack in the address space of the thread's process.
  - their own security context, which can be used for impersonating clients.

## Concurrency
- A **lock** allows only one thread to enter the part that's locked and the lock is not shared with any other processes.
- A **mutex** is the same as a lock but it can be system wide (shared by multiple processes).
- A **semaphore** does the same as a mutex but allows x number of threads to enter, this can be used for example to limit the number of cpu, io or ram intensive tasks running at the same time.

### Example
The producer-consumer problem:

Consider the standard producer-consumer problem. Assume, we have a buffer of 4096 byte length. A producer thread collects the data and writes it to the buffer. A consumer thread processes the collected data from the buffer. Objective is, both the threads should not run at the same time.

**Mutex**:

A mutex provides mutual exclusion, either producer or consumer can have the key (mutex) and proceed with their work. As long as the buffer is filled by producer, the consumer needs to wait, and vice versa. At any point of time, only one thread can work with the entire buffer. The concept can be generalized using semaphore.

**Semaphore**:

A semaphore is a generalized mutex. In lieu of single buffer, we can split the 4 KB buffer into four 1 KB buffers (identical resources). A semaphore can be associated with these four buffers. The consumer and producer can work on different buffers at the same time.

### Concurrency Issues
#### Deadlock
A deadlock is a state in which each member of a group of actions, is waiting for some other member to release a lock.

--> SOLUTION:
deadlock avoidance is to have a **lock hierarchy**. Make sure that **all threads acquire locks** or **other resources** in the **same order**. This avoids the deadlock scenario where thread 1 hold lock A and needs lock B while thread 2 holds lock B and needs lock A. With a lock hierarchy, both threads would have to acquire the locks in the same order (say, A before B).
#### Livelock
A livelock is similar to a deadlock, except that the states of the processes involved in the livelock constantly change with regard to one another, none progressing. Livelock is a special case of resource starvation; the general definition only states that a specific process is not progressing.

##### Example:
A real-world example of livelock occurs when two people meet in a narrow corridor, and each tries to be polite by moving aside to let the other pass, but they end up swaying from side to side without making any progress because they both repeatedly move the same way at the same time.

--> SOLUTION:
Livelock is a risk with some algorithms that detect and recover from deadlock. If more than one process takes action, the deadlock detection algorithm can be repeatedly triggered. This can be avoided by **ensuring that only one process** (chosen randomly or by priority) **takes action**.

#### Lost updates
Two applications, A and B, might both read the same row and calculate new values for one of the columns based on the data that these applications read. If A updates the row and then B also updates the row, A's update lost.

#### Access to uncommitted data.
Application A might update a value, and B might read that value before it is committed. Then, if A backs out of that update, the calculations performed by B might be based on invalid data.

#### Non-repeatable reads.
Application A might read a row before processing other requests. In the meantime, B modifies or deletes the row and commits the change. Later, if A attempts to read the original row again, it sees the modified row or discovers that the original row has been deleted.

#### Phantom reads.
Application A might execute a query that reads a set of rows based on some search criterion. Application B inserts new data or updates existing data that would satisfy application A's query. Application A executes its query again, within the same unit of work, and some additional ("phantom") values are returned.

### Context Switching
In computing, a **context switch** is the process of **storing the state of a process or of a thread**, so that it can be restored and execution resumed from the same point later. This allows multiple processes to share a single CPU, and is an essential feature of a multitasking operating system.

A **context** is the contents of a **CPU's registers** and **program counter** at any point in time. A register is a small amount of very fast memory inside of a CPU (as opposed to the slower RAM main memory outside of the CPU) that is used to speed the execution of computer programs by providing quick access to commonly used values, generally those in the midst of a calculation. A program counter is a specialized register that indicates the position of the CPU in its instruction sequence and which holds either the address of the instruction being executed or the address of the next instruction to be executed, depending on the specific system.

### Scheduling
Schedulers are often implemented so they keep all computer resources busy (as in load balancing), allow multiple users to share system resources effectively, or to achieve a target quality of service. Scheduling is fundamental to computation itself, and an intrinsic part of the execution model of a computer system; the concept of scheduling makes it possible to have computer multitasking with a single central processing unit (CPU).

#### Types
- First come, first served
  - First in, first out (FIFO), also known as first come, first served (FCFS), is the simplest scheduling algorithm. FIFO simply queues processes in the order that they arrive in the ready queue. This is commonly used for a task queue, for example as illustrated in this section.
- Earliest deadline first
- Shortest remaining time first
- Round-robin scheduling
  - The scheduler assigns a fixed time unit per process, and cycles through them. If process completes within that time-slice it gets terminated otherwise it is rescheduled after giving a chance to all other processes.


