# Operating System Fundamentals

## Binary Executable Files

1. The executable files are always in binary form.

2. The binary form executable file is saved as line by line. Each line can have 64 bit or 32 bit value assigned to it.

3. In RAM, the first portion of memory gets occupied by our system processes or system program's processes. (RAM's every cell memory can also be 32 bit or 64 bit)

4. After that comes the executable file part.

5. The executable file (when run), gets assigned line by line in the random access memory's memory cell.

6. RAM has lots of memory cells just like the hard drive but RAM is faster than the hard drive. Each memory cell can be 32 bit or 64 bit.

7. The executable files each line is a binary line which then gets included in the random access memory cell in the time of program execution.

## Processor Architecture

1. The processor has 2 parts:
   - The processing unit
   - The register set

2. Our processor can do only a handful of operations like add, sub, mul, div, and, or, not.

3. There are multiple registers in the register set such as:
   - AX
   - BX
   - CX
   - PX
   - etc.

4. There is a register set whose name is the pointing register.

5. One single register is only one single cell memory thus pointing register is only one memory cell that can store only one binary executable line which can be 32 bit or 64 bit.

6. If the processor's register set's cell memory is 32 bit then the computer is a 32 bit computer.

7. One of the most important registers is the pointing register.

8. Pointing register has only one address assigned to it.

9. The pointing register points to any RAM cell address.

10. When we run an executable file, the first line is held into the part of the RAM.

11. Then the pointing register points to that memory cell's address.

12. Processing unit knows no other information than the pointing register.

13. Then the processing unit executes the value inside the pointing register address.

14. The processor executes code which is why we can work on computers.

---

Everything we saw above was the work of only one process. This is what happened in early computers.

> **Note:** Previously, computers could only do one task at a time. (one program at a time)

## Process Management

1. **Process**: When a pointing register points to a memory cell's address, when a software is loaded in the RAM and the pointing register pointing to an address and the processing unit started executing line by line from the pointing register's pointed values is called a process.

2. Every program has only one process, which is line by line code execution.

3. One process is separated from another process. Process gives a marker to the end point for the pointing register.

4. When a code gets executed, a process is created.

5. **Process == Virtual Computer**

6. Process is made to believe that no one else is running on the computer except itself.

7. Code is the process. Process stays in RAM.

8. A 100 crore cell computer can execute in 1 second.

9. Humans can't perceive anything that happens before 1/10th of a second.

10. People who hear more in one ear and less in the other have more deja vu.

11. Pointing register jumps from one process to another process by switching context or concurrency and saving the previous process's last executed lines information of the register set in the PCB (Process Control Block).

12. Jumping from one process to another process is called context switching.

13. When all the processes are run one time, then the pointing register comes again to the first process and it doesn't know where to start. That's why it takes information from the PCB where the pointing register stopped. Thus continues from the next memory cell address.

14. PCB has relation with the RAM process.

15. Every process has one unique ID in the PCB and in there is the last saved register set information.

16. We are not saving code in the PCB, we are saving the information of the register set.

17. By switching context, a single core can run more than one process. This is called concurrency.

18. Every core has 2 logical processors.

19. When each processor executes only one process then it is called parallel programming or multi programming. Example: 6 CPU running 6 processes.

20. If one processor runs more than one process and the rest processors do only one process, then there will be concurrency in one processor and there will also be parallel programming. Example: logical CPU 6 but processes running 7.

21. Process is the execution of the code. (Process is also the virtual computer)

22. A good computer is the higher generation.

23. Core i3 CPU 5th gen → suppose for 6 processes → 1s → 10^8 → 10 crore → 6 × 10 = 60 crore

24. Core i3 CPU 6th gen → 1s → 10^9 → 100 crore → 6 × 100 = 600 crore

25. Core i4 CPU 5th gen → suppose for 8 processes → 1s → 10^8 → 10 crore → 8 × 10 = 80 crore

26. Another name for thread is virtual processing

> **Note:** Process behaves like a computer, thread behaves like a process but virtually.

1. In thread there is also concurrency, meaning context switching.

2. In thread context switching, not too much information is saved to the PCB except the line which was being executed.

3. In thread concurrency, no time is wasted in context switching, it's so fast!

4. When one process is working on only one task, then it has only one thread.

5. When multiple tasks happen in the process, then multiple threading occurs and thread context switching or virtual process context switching happens.

**JavaScript is a single threaded language but faster than multi-threaded languages!**
