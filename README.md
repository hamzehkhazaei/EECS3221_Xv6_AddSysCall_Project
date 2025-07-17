# Project 1: System Call Implementation for xv6

## Overview

In this project, you will implement a new system call in the xv6 operating system called `getprocs()` that returns information about currently running processes. This project will give you hands-on experience with the complete system call pathway, from user-space programs through the kernel interface to the actual kernel implementation.

## Learning Objectives

- Understand how system calls bridge user space and kernel space
- Learn the anatomy of system call implementation in Unix-like systems
- Practice working with kernel data structures
- Gain experience with kernel programming and debugging

## Requirements

### Core Functionality

Implement a system call with the following signature:
```c
int getprocs(struct procinfo *pinfo, int max_procs)
```

**Parameters:**
- `pinfo`: Pointer to an array of `procinfo` structures (user-space buffer)
- `max_procs`: Maximum number of process entries to return

**Return Value:**
- Number of processes actually returned
- -1 on error (invalid parameters, etc.)

### Process Information Structure

Define a `struct procinfo` that includes:
```c
struct procinfo {
    int pid;           // Process ID
    int ppid;          // Parent process ID
    int state;         // Process state (UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE)
    uint sz;           // Size of process memory (bytes)
    char name[16];     // Process name
};
```

### User Program

Create a user program called `ps` that demonstrates your system call by listing all running processes in a format similar to Unix `ps`.

## Implementation Steps

### Step 1: Define the System Call Interface
1. Add your system call number to `syscall.h`
2. Add the system call declaration to `user.h`
3. Add the system call to `usys.S`

### Step 2: Implement the Kernel Function
1. Add your system call handler to `sysproc.c`
2. Add the function pointer to the syscall table in `syscall.c`
3. Implement the core logic that iterates through the process table

### Step 3: Create the User Program
1. Write a `ps.c` program that calls your system call
2. Add it to the Makefile so it gets built with the system

## Hints and Tips

### Understanding the Process Table
- Look at `struct proc` in `proc.h` to understand available process information
- The global `ptable` contains all process entries
- Remember to acquire the `ptable.lock` when accessing the process table
- Process states are defined as constants in `proc.h`

### Memory Management
- Use `copyout()` to safely copy data from kernel space to user space
- Always validate user pointers before using them
- Consider what happens if the user provides a buffer that's too small

### Key Files to Examine
- `proc.h` and `proc.c`: Process management and data structures
- `syscall.h` and `syscall.c`: System call definitions and dispatch
- `sysproc.c`: Other system call implementations for reference
- `usys.S`: Assembly stubs for system calls

### Debugging Tips
- Use `cprintf()` for kernel debugging output
- The `panic()` function can help catch critical errors
- Test with different numbers of running processes
- Make sure to handle edge cases (no processes, buffer too small, invalid pointers)

### Common Pitfalls
- Forgetting to acquire locks when accessing shared data structures
- Not validating user-space pointers (could cause kernel panic)
- Off-by-one errors when copying process information
- Forgetting to update all necessary files when adding a system call

## Testing Strategy

1. **Basic Functionality**: Run your `ps` program and verify it shows the expected processes
2. **Edge Cases**: Test with `max_procs = 0`, `max_procs = 1`, very large values
3. **Error Handling**: Pass invalid pointers and verify proper error returns
4. **Multiple Processes**: Start several processes and verify that they all appear
5. **Process States**: Create sleeping processes and verify state reporting

## Deliverables
You need to submit a zip file named `xv6-syscall.zip` including the following:
1. Modified xv6 source code with your system call implementation and the `ps.c` test user program.
2. **Demo Video**: A 3-minute video (e.g., Zoom with screen capture and Camera) explaining your code in action.
   
    **Important Note**: At the beginning of the demo, while the camera is on, introduce yourself with your student number, then start the demo by using the screen capture feature.
   
4. A `readme.txt` file including:
   - Your implementation approach
   - Any challenges encountered and how you solved them
   - Test cases you used to verify correctness

```
## 📤 Your zip file should have a structure like below:

📦 xv6-syscall.zip  
│   ├── 📂 xv6-public  
│   └── 📝 Include ALL modifications and test programs  
├── 🎥 demo_video.mp4  
│   ├── ⏱️ Duration: 3 minutes maximum  
│   ├── 🖥️ Screen capture with camera  
│   └── 📢 Audio explanation of implementation  
└── 📖 readme.txt  
    ├── 💡 Implementation explanation  
    ├── 🔍 Design decisions  
    ├── 🧪 Testing approach  
    └── 📚 Usage instructions
```


## Grading Criteria

- **Correctness (40%)**: System call works as specified
- **Code Quality (30%)**: Clean, well-commented code following kernel conventions
- **Error Handling (20%)**: Proper validation and error reporting
- **Testing (10%)**: Comprehensive testing and demonstration

## Getting Started

1. Make sure you can build and run the xv6 OS
2. Study existing system call implementations in `sysproc.c`
3. Start by adding the system call interface before implementing functionality
4. Test incrementally - get a simple version working before adding complexity

Good luck! Remember that kernel programming requires meticulous attention to detail, as even minor bugs can crash the entire system.

**Instructor:** Hamzeh Khazaei
