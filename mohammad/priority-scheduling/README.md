# Priority Scheduling – Non-Preemptive

**Student:** Mohammad  
**Algorithm:** Priority Scheduling (Non-Preemptive)  
**Course:** CS11335 Operating Systems – Princess Sumaya University

---

## What is Priority Scheduling?

Priority Scheduling is a CPU scheduling algorithm where each process is assigned a priority value.  
The CPU chooses the process with the highest priority among the processes that have already arrived.

In this implementation, the priority convention is:

```
1 = Highest Priority
Larger number = Lower Priority
```

Since this version is non-preemptive, once a process starts executing, it cannot be interrupted. It continues running until it finishes, even if a higher-priority process arrives while it is running.

---

## Key Metrics

| Metric           | Formula                              |
|------------------|--------------------------------------|
| Completion Time  | Time when the process finishes       |
| Turnaround Time  | Completion Time − Arrival Time       |
| Waiting Time     | Turnaround Time − Burst Time         |

---

## Code Structure

```
priority.cpp
└── main()
    ├── read input       → ask user for number of processes
    │                       then arrival time, burst time, and priority for each process
    ├── priority loop    → select the available process with the highest priority
    │                       if priorities are equal, choose the earlier arrival time
    │                       if no process has arrived, CPU time increases by 1
    └── print results    → Gantt chart + process table + averages
```

---

## How to Compile & Run

**Requires:** g++ compiler (MinGW on Windows, or g++ on Linux/macOS)

```bash
cd mohammad/priority-scheduling/code
g++ -o priority priority.cpp
./priority          # Linux/macOS
priority.exe        # Windows
```

> No compiler installed? Paste the code into <https://www.onlinegdb.com/online_c++_compiler> and click **Run**.

---

## How to Use the Program

When the program runs, it will prompt you step-by-step:

```
Enter number of processes: 5

P1
Arrival Time: 0
Burst Time: 8
Priority (1 = highest): 3

P2
Arrival Time: 1
Burst Time: 4
Priority (1 = highest): 1
...
```

Just type each number and press **Enter** after each one.

---

## Test Cases

Below are the shared CPU scheduling test cases used by the group.  
Each test case lists the values to type in order, one per line, and the expected output.

---

### Test Case 1 – General Mixed Case

A balanced set of processes with staggered arrivals and mixed burst times.  
This represents a typical real-world workload.

**Input:**
```
5
0
8
3
1
4
1
2
9
4
3
5
2
4
2
5
```

**Expected Output:**
```
Gantt Chart: P1 P2 P4 P3 P5

Process Arrival Burst Priority Completion Waiting Turnaround
P1      0       8     3        8          0       8
P2      1       4     1        12         7       11
P3      2       9     4        26         15      24
P4      3       5     2        17         9       14
P5      4       2     5        28         22      24

Average Waiting Time = 10.6
Average Turnaround Time = 16.2
```

---

### Test Case 2 – Convoy Effect

One long process arrives first, followed by several short ones.  
This highlights the weakness of non-preemptive algorithms because the first long process cannot be interrupted.

**Input:**
```
4
0
20
4
1
3
2
2
3
1
3
3
3
```

**Expected Output:**
```
Gantt Chart: P1 P3 P2 P4

Process Arrival Burst Priority Completion Waiting Turnaround
P1      0       20    4        20         0       20
P2      1       3     2        26         22      25
P3      2       3     1        23         18      21
P4      3       3     3        29         23      26

Average Waiting Time = 15.75
Average Turnaround Time = 23
```

---

### Test Case 3 – CPU Idle Gaps

Processes arrive with large gaps between them, causing the CPU to sit idle in between.  
This shows how the algorithm handles waiting periods when no process is available.

**Input:**
```
4
0
5
2
8
3
1
12
7
3
20
4
2
```

**Expected Output:**
```
Gantt Chart: P1 P2 P3 P4

Process Arrival Burst Priority Completion Waiting Turnaround
P1      0       5     2        5          0       5
P2      8       3     1        11         0       3
P3      12      7     3        19         0       7
P4      20      4     2        24         0       4

Average Waiting Time = 0
Average Turnaround Time = 4.75
```

---

## Notes About the Algorithm

- The process with the lowest priority number runs first.
- If two processes have the same priority, the process with the earlier arrival time runs first.
- The algorithm is non-preemptive, so a running process cannot be interrupted.
- If no process has arrived yet, the CPU stays idle and time increases by one unit.
- The Gantt Chart shows the execution order of processes.
- The final table is printed in process number order.

---

## Starvation

Priority Scheduling can cause starvation.  
This means that low-priority processes may wait for a long time if higher-priority processes keep arriving.

In this program, all entered processes eventually execute because the number of processes is fixed.  
However, in a real operating system where new processes may continuously arrive, starvation can happen.

A common solution is **aging**, where the priority of a waiting process gradually improves over time.

---

## References

- Silberschatz, A., Galvin, P., & Gagne, G. (2018). *Operating System Concepts* (10th ed.). Wiley.
- Priority scheduling – Wikipedia
