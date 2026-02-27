# CPU Scheduling Algorithms - C++ Implementation

A comprehensive implementation of 8 CPU scheduling algorithms in C++, simulating operating system process management with timeline visualization and statistical analysis.

[![Language](https://img.shields.io/badge/Language-C%2B%2B-blue.svg)](https://isocpp.org/)
[![Platform](https://img.shields.io/badge/Platform-Windows-lightgrey.svg)](https://www.microsoft.com/windows)
[![Compiler](https://img.shields.io/badge/Compiler-MinGW%2Fg%2B%2B-green.svg)](https://mingw.org/)

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Algorithms Implemented](#algorithms-implemented)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Input Format](#input-format)
- [Output Format](#output-format)
- [Algorithm Details](#algorithm-details)
- [Performance Comparison](#performance-comparison)
- [Test Cases](#test-cases)
- [Project Structure](#project-structure)
- [Technical Specifications](#technical-specifications)

---

## Overview

This project demonstrates how operating systems manage CPU scheduling through simulation of 8 different scheduling algorithms. It provides both **trace mode** (timeline visualization) and **stats mode** (statistical analysis) to understand process execution behavior, waiting times, and turnaround times.

### What This Project Does

- ✅ Simulates CPU scheduling for multiple processes
- ✅ Supports 8 different scheduling algorithms
- ✅ Visualizes process execution timelines
- ✅ Calculates performance metrics (turnaround time, normalized turnaround)
- ✅ Handles both preemptive and non-preemptive scheduling
- ✅ Includes 24 comprehensive test cases

### Real-World Applications

- **Operating Systems**: Understanding how Windows, Linux, macOS manage processes
- **System Performance**: Analyzing and optimizing CPU utilization
- **Interview Preparation**: Common topic in OS and systems programming interviews
- **Educational**: Learning fundamental OS scheduling concepts

---

## Features

| Feature | Description |
|---------|-------------|
| **8 Algorithms** | FCFS, Round Robin, SPN, SRT, HRRN, FB-1, FB-2i, Aging |
| **Visualization** | Timeline showing process execution with `*` (running) and `.` (waiting) |
| **Statistics** | Turnaround time, normalized turnaround calculations |
| **Flexible Input** | Standard input (stdin) for easy testing |
| **Comprehensive Tests** | 24 test cases covering all algorithms |
| **Cross-Platform** | Windows with MinGW (easily portable to Linux/macOS) |

---

## Algorithms Implemented

### Quick Comparison

| Algorithm | Type | Preemptive | Starvation Risk | Best Use Case |
|-----------|------|------------|-----------------|---------------|
| **FCFS** | Non-preemptive | No | None | Batch processing |
| **Round Robin** | Preemptive | Yes | None | Time-sharing systems |
| **SPN** | Non-preemptive | No | High (long jobs) | Throughput optimization |
| **SRT** | Preemptive | Yes | High (long jobs) | Minimize turnaround time |
| **HRRN** | Non-preemptive | No | Low | Balanced performance |
| **FB-1** | Preemptive | Yes | Low | Interactive systems |
| **FB-2i** | Preemptive | Yes | Low | Mixed workloads |
| **Aging** | Preemptive | Yes | None | Priority-based with fairness |

---

## Installation & Setup

### Prerequisites
- **OS**: Windows 10/11
- **Compiler**: MinGW (g++)
- **Tools**: PowerShell or Command Prompt

### Step 1: Install MinGW Compiler

#### Option A: Using Chocolatey (Recommended)

1. Open PowerShell as **Administrator**
2. Install Chocolatey (if not already installed):
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

3. Install MinGW:
```powershell
choco install mingw -y
```

4. Verify installation:
```powershell
g++ --version
```

#### Option B: Manual Installation

1. Download MinGW: [sourceforge.net/projects/mingw](https://sourceforge.net/projects/mingw/)
2. Install and add to PATH: `C:\MinGW\bin`
3. Restart terminal and verify with `g++ --version`

### Step 2: Clone or Download Repository

Download the project files to your system, for example:
```powershell
cd "d:\OS project\CPU-Scheduling-Algorithms"
```

### Step 3: Compile the Project

```powershell
g++ -o scheduler.exe main.cpp
```

**Or use the makefile:**
```powershell
mingw32-make
```

This creates `scheduler.exe` (or `main.exe` with makefile).

---

## Usage

### Running with Test Cases

#### Method 1: PowerShell (Recommended)
```powershell
Get-Content testcases\01a-input.txt | .\scheduler.exe
```

#### Method 2: Command Prompt
```cmd
scheduler.exe < testcases\01a-input.txt
```

#### Method 3: Interactive Input
```powershell
.\scheduler.exe
# Then type input manually, press Ctrl+Z and Enter when done
```

### Run All Test Cases

```powershell
Get-ChildItem testcases\*-input.txt | ForEach-Object {
    Write-Host "`n=== Testing: $($_.Name) ===" -ForegroundColor Cyan
    Get-Content $_ | .\scheduler.exe
}
```

### Compare Output with Expected

```powershell
.\scheduler.exe < testcases\01a-input.txt > output.txt
fc output.txt testcases\01a-output.txt
```

---

## Input Format

The input consists of:

1. **Line 1**: Mode - `trace` or `stats`
   - `trace`: Shows timeline visualization
   - `stats`: Shows statistical analysis

2. **Line 2**: Comma-separated algorithm list
   - Format: `algorithm_id-quantum` (quantum only for algorithms that need it)
   - Example: `1,2-4,3,8-1` (FCFS, RR with q=4, SPN, Aging with q=1)

3. **Line 3**: Last instant (timeline end time)

4. **Line 4**: Number of processes

5. **Lines 5+**: Process descriptions (one per line)
   - For algorithms 1-7: `ProcessName,ArrivalTime,ServiceTime`
   - For algorithm 8 (Aging): `ProcessName,ArrivalTime,Priority`

### Algorithm IDs

| ID | Algorithm | Quantum Parameter |
|----|-----------|-------------------|
| 1 | FCFS (First Come First Serve) | N/A |
| 2-q | Round Robin | q (time quantum) |
| 3 | SPN (Shortest Process Next) | N/A |
| 4 | SRT (Shortest Remaining Time) | N/A |
| 5 | HRRN (Highest Response Ratio Next) | N/A |
| 6 | FB-1 (Feedback, quantum=1) | N/A |
| 7 | FB-2i (Feedback, quantum=2^i) | N/A |
| 8-q | Aging | q (time quantum) |

### Example Input

```
trace
1,2-2
20
3
A,0,5
B,2,3
C,4,2
```

This runs FCFS and Round Robin (quantum=2) on 3 processes, displaying results up to time 20.

---

## Output Format

### Trace Mode

Shows timeline with:
- `*` = Process is executing
- `.` = Process is waiting
- ` ` (space) = Process hasn't arrived or has finished

```
FCFS   0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 
------------------------------------------------
A     |*|*|*|*|*| | | | | | | | | | | | | | | | 
B     | | |.|.|.|*|*|*| | | | | | | | | | | | | 
C     | | | | |.|.|.|.|*|*| | | | | | | | | | | 
------------------------------------------------
```

### Stats Mode

Shows statistical information:
- **Arrival**: When process entered the ready queue
- **Service**: CPU time needed
- **Finish**: When process completed
- **Turnaround**: Finish - Arrival
- **NormTurn**: Turnaround / Service (efficiency metric)

```
FCFS        A    B    C    
Arrival     0    2    4    
Service     5    3    2    
Finish      5    8   10    
Turnaround  5    6    6    
NormTurn    1.00 2.00 3.00 
```

---

## Algorithm Details

### 1. First Come First Serve (FCFS)
- First Come First Served (FCFS) is a scheduling algorithm in which the process that arrives first is executed first. It is a simple and easy-to-understand algorithm, but it can lead to poor performance if there are processes with long burst times. This algorithm does not have any mechanism for prioritizing processes, so it is considered a non-preemptive algorithm. In FCFS scheduling, the process that arrives first is executed first, regardless of its burst time or priority. This can lead to poor performance, as longer running processes will block shorter ones from being executed. It is commonly used in batch systems where the order of the processes is important.

### Round Robin with varying time quantum (RR)
- Round Robin (RR) with variable quantum is a scheduling algorithm that uses a time-sharing approach to divide CPU time among processes. In this version of RR, the quantum (time slice) is not fixed and can be adjusted depending on the requirements of the processes. This allows processes with shorter burst times to be given smaller quanta and vice versa.

- The algorithm works by maintaining a queue of processes, where each process is given a quantum of time to execute on the CPU. When a process's quantum expires, it is moved to the back of the queue, and the next process in the queue is given a quantum of time to execute.

- The variable quantum allows the algorithm to be more efficient as it allows the CPU to spend more time on shorter processes and less time on longer ones. This can help to minimize the average waiting time for processes. Additionally, it also helps to avoid the issue of starvation, which occurs when a process with a long burst time prevents other processes from executing.

### Shortest Process Next (SPN)
- Shortest Process Next (SPN) is a scheduling algorithm that prioritizes the execution of processes based on their burst time, or the amount of time they need to complete their task. It is a non-preemptive algorithm which means that once a process starts executing, it runs until completion or until it enters a waiting state.

- The algorithm maintains a queue of processes, where each process is given a burst time when it arrives. The process with the shortest burst time is executed first, and as new processes arrive, they are added to the queue and sorted based on their burst time. The process with the shortest burst time will always be at the front of the queue, and thus will always be executed next.

- This algorithm can be beneficial in situations where the objective is to minimize the average waiting time for processes, since shorter processes will be executed first, and thus will spend less time waiting in the queue. However, it can lead to longer running processes being blocked by shorter ones, which can negatively impact the overall performance of the system.

- In summary, SPN is a scheduling algorithm that prioritizes the execution of processes based on their burst time, it's non-preemptive and it's commonly used in situations where the objective is to minimize the average waiting time for processes.
### Shortest Remaining Time (SRT)
- Shortest Remaining Time Next (SRT) is a scheduling algorithm that is similar to the Shortest Process Next (SPN) algorithm, but it is a preemptive algorithm. This means that once a process starts executing, it can be interrupted by a new process with a shorter remaining time.

- The algorithm maintains a queue of processes, where each process is given a burst time when it arrives. The process with the shortest remaining time is executed first, and as new processes arrive, they are added to the queue and sorted based on their remaining time. The process with the shortest remaining time will always be at the front of the queue, and thus will always be executed next.

- This algorithm can be beneficial in situations where the objective is to minimize the average waiting time for processes, since shorter processes will be executed first, and thus will spend less time waiting in the queue. Additionally, it can be useful in situations where the burst time of processes is not known in advance, since the algorithm can adapt to changes in the remaining time as the process is executing.

- In summary, SRT is a scheduling algorithm that prioritizes the execution of processes based on their remaining time, it's a preemptive algorithm, which means that it can interrupt a process that's already executing if a new process with a shorter remaining time arrives and it's commonly used in situations where the objective is to minimize the average waiting time for processes and burst time is not known in advance.

### Highest Response Ratio Next (HRRN)

- Highest Response Ratio Next (HRRN) is a scheduling algorithm that prioritizes the execution of processes based on their response ratio. It is a non-preemptive algorithm which means that once a process starts executing, it runs until completion or until it enters a waiting state.

- The response ratio is calculated by taking the ratio of the waiting time of a process and its burst time. The process with the highest response ratio is executed first, and as new processes arrive, they are added to the queue and sorted based on their response ratio. The process with the highest response ratio will always be at the front of the queue, and thus will always be executed next.

- This algorithm can be beneficial in situations where the objective is to minimize the average waiting time for processes, since processes with longer waiting times will be executed first, and thus will spend less time waiting in the queue. Additionally, it can be useful in situations where the burst time of processes is not known in advance, since the algorithm can adapt to changes in the waiting time as the process is executing.

- In summary, HRRN is a scheduling algorithm that prioritizes the execution of processes based on their response ratio, it's non-preemptive and it's commonly used in situations where the objective is to minimize the average waiting time for processes and burst time is not known in advance.

### Feedback (FB)

- Feedback is a scheduling algorithm that allocates CPU time to different processes based on their priority level. It is a multi-level priority algorithm that uses multiple priority queues, each with a different priority level.

- Processes with higher priority levels are executed first, and as new processes arrive, they are added to the appropriate priority queue based on their priority level. When a process completes execution, it is moved to the next lower priority queue.

- This algorithm can be beneficial in situations where the system needs to handle a mix of short and long-running processes, as well as processes with varying priority levels. By having multiple priority queues, it ensures that processes with higher priority levels are executed first, while also allowing lower-priority processes to eventually be executed.

- In summary, Feedback is a scheduling algorithm that allocates CPU time based on priority levels, it uses multiple priority queues with different levels of priority, processes with higher priority levels are executed first and when process completes execution, it is moved to the next lower priority queue, it's commonly used in situations where system needs to handle a mix of short and long-running processes, as well as processes with varying priority levels.

### Feedback with varying time quantum (FBV)
- Same as [Feedback](#feedback-fb) but with varying time quantum.
- Feedback with varying time quantum also uses multiple priority queues and assigns a different time quantum for each priority level, it allows the algorithm to be more efficient by spending more time on higher-priority processes and less time on lower-priority processes.

### Aging

- Xinu is an operating system developed at Purdue University. The scheduling invariant in Xinu assumes that at any
time, the highest priority process eligible for CPU service is executing, with round-robin scheduling for processes of
equal priority. Under this scheduling policy, the processes with the highest priority will always be executing. As a
result, all the processes with lower priority will never get CPU time. As a result, starvation is produced in Xinu when
we have two or more processes eligible for execution that have different priorities. For ease of discussion, we call the
set of processes in the ready list and the current process as the eligible processes.

- To overcome starvation, an aging scheduler may be used. On each rescheduling operation, a timeout for instance, the
scheduler increases the priority of all the ready processes by a constant number. This avoids starvation as each ready
process can be passed over by the scheduler only a finite number of times before it has the highest priority.

- Each process has an initial priority that is assigned to it at process creation. Every time the scheduler is called it takes
the following steps.
    - The priority of the current process is set to the initial priority assigned to it.
    - The priorities of all the ready processes (not the current process) are incremented by 1.
    - The scheduler choses the highest priority process from among all the eligible processes.

- Note that during each call to the scheduler, the complete ready list has to be traversed.

## Installation and Running

### Prerequisites
- Windows with PowerShell
- MinGW (g++ compiler)

### Step 1: Install MinGW Compiler
1. Open PowerShell **as Administrator**
2. Install Chocolatey package manager (if not already installed):
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   ```
3. Install MinGW:
   ```powershell
   choco install mingw -y
   ```
4. Close and reopen PowerShell to refresh environment variables

### Step 2: Compile the Project
```powershell
cd "d:\OS project\CPU-Scheduling-Algorithms"
g++ -o scheduler.exe main.cpp
```

### Step 3: Run the Program
Run with a test case:
```powershell
Get-Content testcases\01a-input.txt | .\scheduler.exe
```

Or use command prompt for input redirection:
```cmd
scheduler.exe < testcases\01a-input.txt
```

### Step 4: Test Multiple Cases
Run all test cases at once:
```powershell
Get-ChildItem testcases\*-input.txt | ForEach-Object {
    Write-Host "`n=== Testing: $($_.Name) ===" -ForegroundColor Cyan
    Get-Content $_ | .\scheduler.exe
}
```

## Input Format
- Line 1: "trace" or "stats"
- Line 2: a comma-separated list telling which CPU scheduling policies to be analyzed/visualized along with
their parameters, if applicable. Each algorithm is represented by a number as listed in the
introduction section and as shown in the attached testcases.
Round Robin and Aging have a parameter specifying the quantum q to be used. Therefore, a policy
entered as 2-4 means Round Robin with q=4. Also, policy 8-1 means Aging with q=1.
 1. FCFS (First Come First Serve)
 2. RR (Round Robin)
 3. SPN (Shortest Process Next)
 4. SRT (Shortest Remaining Time)
 5. HRRN (Highest Response Ratio Next)
 6. FB-1, (Feedback where all queues have q=1)
 7. FB-2i, (Feedback where q= 2i)
 8. Aging
- Line 3: An integer specifying the last instant to be used in your simulation and to be shown on the timeline.
- Line 4: An integer specifying the number of processes to be simulated.
- Line 5: Start of description of processes. Each process is to be described on a separate line. For algorithms 1 through 7, each process is described using a comma-separated list specifying:

    1- String specifying a process name\
    2- Arrival Time\
    3- Service Time

- **Note:** For Aging algorithm (algorithm 8), each process is described using a comma-separated list specifying:

    1- String specifying a process name\
    2- Arrival Time\
    3- Priority
- Processes are assumed to be sorted based on the arrival time. If two processes have the same arrival time, then the one with the lower priority is assumed to arrive first.
- Check the testcases folder for examples and expected outputs.
---

## Performance Comparison

### Algorithm Characteristics

| Algorithm | Type | Time Complexity | Space Complexity | Starvation Risk | Best Use Case |
|-----------|------|-----------------|------------------|-----------------|---------------|
| **FCFS** | Non-preemptive | O(n) | O(n×t) | None | Batch processing |
| **RR** | Preemptive | O(n×t) | O(n×t) | None | Time-sharing systems |
| **SPN** | Non-preemptive | O(n²) | O(n×t) | High | Known burst times |
| **SRT** | Preemptive | O(n×t) | O(n×t) | High | Minimize avg wait time |
| **HRRN** | Non-preemptive | O(n²) | O(n×t) | Low | Balanced performance |
| **FB-1** | Preemptive | O(n×t×q) | O(n×t) | Low | Unknown workloads |
| **FB-2i** | Preemptive | O(n×t×log q) | O(n×t) | Low | Mixed workloads |
| **Aging** | Preemptive | O(n²×t) | O(n×t) | None | Priority with fairness |

*where n = number of processes, t = total time units, q = number of queues*

### Performance Metrics

**Turnaround Time** = Finish Time - Arrival Time  
**Normalized Turnaround** = Turnaround Time / Service Time  
**Waiting Time** = Turnaround Time - Service Time

#### Pros & Cons Summary

**FCFS**
- ✅ Simple, no starvation, minimal overhead
- ❌ Convoy effect, poor for interactive systems

**Round Robin**
- ✅ Fair, good response time, no starvation
- ❌ High context switch overhead, performance depends on quantum

**SPN**
- ✅ Optimal avg waiting time (among non-preemptive)
- ❌ Long processes may starve, requires burst time knowledge

**SRT**
- ✅ Optimal avg waiting time (among all algorithms)
- ❌ High overhead, long processes may starve

**HRRN**
- ✅ Prevents starvation, balances short/long processes
- ❌ O(n²) overhead per scheduling decision

**FB-1**
- ✅ No prior knowledge needed, adapts to process behavior
- ❌ Very high context switch overhead

**FB-2i**
- ✅ Better for mixed workloads, lower overhead than FB-1
- ❌ More complex, tuning required

**Aging**
- ✅ Prevents priority inversion and starvation
- ❌ Priority calculation overhead

---

## Test Cases

The project includes **24 comprehensive test cases** (12 algorithms × 2 modes each):

### Test Case Structure
```
testcases/
├── 01a-input.txt   → FCFS trace mode
├── 01a-output.txt  → Expected output
├── 01b-input.txt   → FCFS stats mode  
├── 01b-output.txt  → Expected output
├── 02a-input.txt   → Round Robin trace
...
└── 12a-output.txt  → Multiple algorithms
```

### Running Tests

**Single test:**
```powershell
Get-Content testcases\01a-input.txt | .\main.exe
```

**All tests with validation:**
```powershell
$tests = Get-ChildItem testcases\*-input.txt
foreach ($test in $tests) {
    $output = $test.Name -replace '-input', '-output'
    Write-Host "Testing: $($test.Name)" -ForegroundColor Cyan
    Get-Content $test | .\main.exe > temp.txt
    $match = Compare-Object (Get-Content temp.txt) (Get-Content "testcases\$output")
    if ($match) {
        Write-Host "  FAILED" -ForegroundColor Red
    } else {
        Write-Host "  PASSED" -ForegroundColor Green
    }
}
Remove-Item temp.txt
```

### Test Coverage

| Test | Algorithm | Processes | Features Tested |
|------|-----------|-----------|-----------------|
| 01 | FCFS | 5 | Basic non-preemptive |
| 02 | RR (q=4) | 5 | Time-sharing, quantum=4 |
| 03 | SPN | 5 | Shortest job first |
| 04 | SRT | 5 | Preemptive shortest remaining |
| 05 | HRRN | 5 | Response ratio calculation |
| 06 | FB-1 | 5 | Multi-level feedback, q=1 |
| 07 | FB-2i | 5 | Exponential quantum growth |
| 08 | Aging | 5 | Priority aging mechanism |
| 09-12 | Multiple | Varies | Comparison scenarios |

---

## Project Structure

```
CPU-Scheduling-Algorithms/
├── main.cpp              # Core implementation (~540 lines)
│   ├── Algorithm implementations (FCFS, RR, SPN, SRT, HRRN, FB, Aging)
│   ├── Timeline management functions
│   ├── Statistics calculation
│   └── Output formatting
│
├── parser.h              # Input parsing utilities
│   ├── parse() - Main parser function
│   ├── parse_algorithms() - Algorithm string parser
│   └── Global variable declarations
│
├── makefile              # Build configuration
│   └── Compiles with g++ -std=c++11
│
├── testcases/            # Comprehensive test suite
│   ├── *-input.txt      # 24 input files
│   └── *-output.txt     # 24 expected output files
│
└── README.md            # This file
```

### Key Implementation Details

**Data Structures:**
- `vector<vector<char>> timeline` - 2D grid for process visualization
- `vector<tuple<string,int,int>> processes` - Process information
- `unordered_map<string,int> processToIndex` - Name to index mapping
- `vector<int> finishTime, turnAroundTime` - Metrics storage
- `vector<float> normTurn` - Normalized turnaround times

**Algorithm Structure:**
```cpp
void execute_algorithm(char algo_id, int quantum) {
    switch(algo_id) {
        case '1': firstComeFirstServe(); break;
        case '2': roundRobin(quantum); break;
        case '3': shortestProcessNext(); break;
        case '4': shortestRemainingTime(); break;
        case '5': highestResponseRatioNext(); break;
        case '6': feedback_1(); break;
        case '7': feedback_2i(); break;
        case '8': aging(quantum); break;
    }
}
```

---

## Technical Specifications

**Language:** C++11  
**Compiler:** g++ (MinGW on Windows)  
**Lines of Code:** ~640 total (540 main.cpp + 100 parser.h)  
**Memory Usage:** O(n × t) where n=processes, t=time units  
**No External Dependencies:** Uses only C++ standard library

### Build Details

```bash
# Compilation command
g++ -o main.exe main.cpp -std=c++11

# Or using make
mingw32-make
```

**Supported Flags:**
- `-std=c++11` - Requires C++11 features
- `-O2` - Optimization (optional)
- `-Wall` - Show all warnings (recommended for development)

---

## Contributing & Extending

### Adding a New Algorithm

1. **Add algorithm function in main.cpp:**
```cpp
void myNewAlgorithm() {
    // Implement scheduling logic
    // Update timeline with '*' for executing, '.' for waiting
    // Calculate finish times
}
```

2. **Update execute_algorithm() switch:**
```cpp
case '9': myNewAlgorithm(); break;
```

3. **Create test cases:**
   - Add `testcases/13a-input.txt` and `testcases/13a-output.txt`

### Common Modifications

**Changing Time Granularity:** Adjust `last_instant` in input  
**Adding Process Attributes:** Extend process tuple structure  
**Custom Output Format:** Modify `printTimeline()` or `printStats()`

---

## Common Issues & Solutions

**Issue: `g++` not recognized**
- Solution: Install MinGW via Chocolatey or add to PATH

**Issue: Incorrect output spacing**
- Solution: Ensure console uses fixed-width font (Consolas, Courier New)

**Issue: Process names not aligning**
- Solution: Keep process names short (1-2 characters work best)

**Issue: Timeline too wide**
- Solution: Reduce `last_instant` value in input

---

## License & Academic Use

This project was developed for educational purposes to demonstrate CPU scheduling algorithms. Feel free to use it for:
- Learning operating system concepts
- Academic coursework and projects  
- Teaching algorithm implementations
- Interview preparation

When using this project, please maintain proper attribution and academic integrity guidelines.

---

## Related Projects

**Python Implementation:** See the companion Python version in [`CPU-Scheduling-Python/`](../CPU-Scheduling-Python/) for:
- Object-oriented design with inheritance
- Cross-platform compatibility (Linux/macOS/Windows)
- Cleaner, more modular code structure
- Easier to extend and customize

---

**Last Updated:** February 2026  
**Author:** OS Course Project Implementation