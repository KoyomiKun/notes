# Concept of Operating System

## Chapter 5 : CPU Scheduling

### 5.1 Basic Concepts

Kernel Thread --> User Thread

This chapter talks about the alogrithm of kernel thread

***Assuming that we know HOW LONG one thread takes*** 

### 5.2 CPU Scheduler

Long / short / mid term scheduler:

- Long term scheduler moves a process from storage pool into the wating queue

- Short term scheduler shedules a process from queue for execution

Concern about the *short term scheduler* .


**Four different kinds of scheduling:**


![Process_states](/home/komikun/Pictures/screenshoots/2020-09-29_17-37-37_screenshot.png) 

1. running -> waiting 

2. running -> ready

3. waiting -> ready

4. terminates

1 and 4 is nonpreemptive, others are preemptive

### 5.3 Dispatcher

save old PCB and create new PCB

Dispatcher Lantency must less than Process Running Time

### 5.4 Scheduling Criteris

1. CPU utilization

2. Throughput 

***3. Time around time : for a special process***

***4. Waiting time : for a special process***

5. Response Time : hum / computer 

### 5.5 Alogrithm


#### 5.5.1 FCFS

First come first serve

Gantt Chart:

![FCFS](/home/komikun/Pictures/screenshoots/2020-09-29_16-05-48_screenshot.png) 

Two Assumption:

1. Know the time process takes

2. No switch time

#### 5.5.2 SJF

Shortest Job Serve, the best way to minimize average waiting time ***Proof?*** 

Dynamic Alogrithm: Because ready queue keeps changing

1. Nonpreemptive

![NP-SJF](/home/komikun/Pictures/screenshoots/2020-09-29_16-11-45_screenshot.png) 


2. Preemptive

![P-SJF](/home/komikun/Pictures/screenshoots/2020-09-29_16-14-18_screenshot.png) 


***Prediction of Length of Next CPU Burst*** 

NO WAY, But can be estimated

- $t_n$ is the actual length of n-th CPU burst

- $\tau _ {n+1}$ is the predicted value for next CPU burst

- $\alpha, 0\le \alpha \le 1$ : aging factor

- Then $\to _ {n+1} = \alpha t_n + (1-\alpha) \tau_n$ 

if $\alpha=0 \ or\ a =1$ ...

#### 5.5.3 Priority Scheduling

As time pass by, we rising the priority of process that waiting for a long time.

#### 5.5.4 Round Robin (RR)

divide time into several time pieces, each process takes a piece and switch to next process. 

***No Process waits more than (n-1)q time units*** 

Confirm the response time.

![RR](/home/komikun/Pictures/screenshoots/2020-09-29_16-27-28_screenshot.png) 


if the pieces takes too long, this alogrithm will turn to FCFS.

0 8
1 4
2 5
3 3

1. FCFS = (7 + 10 + 14) / 4 = 31 / 4

2. NP-SJF = (5 + 10 + 13) / 4 = 28 / 4
   P-SJF = 

3. 59 / 4


### 5.6 Multilevel Queue Scheduling

foreground - RR (response time)

background - FCFS

Upgrade and Demote? When ?

...

### 5.7 Multiple-Processor Scheduling

Asymmetric : main CPU with assistant CPU

symmetric multipleprocessor(SMP) : same level CPUs

### 5.8 Real-Time Scheduling

1. Hard real-time: Must be completed Before DDL

2. Firm: Must, but if it is not important, accept

2. Soft real-time: Accept after DDL. But reduce some marks


## Chapter 6 : Process Synchronization



