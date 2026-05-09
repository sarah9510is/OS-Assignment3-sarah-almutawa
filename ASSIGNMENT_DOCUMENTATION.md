# Assignment 3 - Complete Documentation

**Student Name**: [sarah abdullah almotawa]  
**Student ID**: [445052089]  
**Date Submitted**: [5/8/2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [April 30, 2026 – 4:30 PM]
**What I implemented**: I thoroughly examined the scheduler simulation that was supplied and noted every shared resource that was used by several threads. I examined where race situations might arise and how concurrent processes operate.

**Challenges encountered**: recognizing risky activities and comprehending how threads interact with shared variables.

**How I solved it**: examined the class notes and contrasted synchronization instances with the code.

**Testing approach**: To see behavior and output consistency, the application was run multiple times.

**Time spent**: 1 hour

---

### Entry 2 - [April 30, 2026 – 5:30 PM]
**What I implemented**: ReentrantLock was put into place, protecting shared counters and waiting time computations.

**Challenges encountered**: Ensuring locks are always released safely to avoid deadlocks.

**How I solved it**: Used try–finally blocks in all critical sections.

**Testing approach**: Tested program repeatedly and confirmed no runtime errors.

**Time spent**: 2hours

---

### Entry 3 - [May 1, 2026 – 2:00 PM]
**What I implemented**: Protected the executionLog ArrayList using locks.

**Challenges encountered**: Understanding why ArrayList is not thread-safe.

**How I solved it**: Researched ConcurrentModificationException and thread safety.

**Testing approach**: Ran program multiple times to confirm stability.

**Time spent**: 1hour

---

### Entry 4 - [May 1, 2026 – 3:00 PM]
**What I implemented**: Implemented CPU semaphore to simulate single CPU access.

**Challenges encountered**: Understanding difference between lock and semaphore.

**How I solved it**: Applied acquire/release inside try–finally blocks.

**Testing approach**: Verified processes execute sequentially.

**Time spent**: 2hour

---

### Entry 5 - [May 2, 2026 – 8:00 PM]
**What I implemented**: Final testing, statistics verification, and documentation writing.

**Challenges encountered**: Ensuring consistent results across multiple runs.

**How I solved it**: Performed repeated testing and verified output correctness.

**Testing approach**: Executed program 5+ times.

**Time spent**: 1.5hours

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
The original code contained two race conditions:

Multiple threads may update shared counters like contextSwitchCount and totalWaitingTime at the same time, resulting in missed updates and inaccurate data.
A ConcurrentModificationException or damaged data could result from multiple threads accessing the executionLog ArrayList concurrently.

In the absence of synchronization, threads might read and write shared variables simultaneously, leading to unexpected outcomes.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

ReentrantLock provides mutual exclusion for critical sections, ensuring only one thread modifies shared data at a time.
Semaphore controls access to a limited resource.

In my implementation:

ReentrantLock was used to protect shared counters and executionLog.
Semaphore(1) was used to simulate a single CPU core, ensuring only one process executes at a time.
---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
Deadlock occurs when threads wait forever for resources held by each other.

Prevention techniques used:

Using a single global lock.
Always releasing locks using try–finally.

This guarantees resources are always released

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I used one global lock (coarse-grained locking).

Reason:
The operations are very short and simple. Using multiple locks would add unnecessary complexity.

Trade-offs:

Coarse-grained: simpler but less concurrency.
Fine-grained: more concurrency but complex and error-prone.

Given the small workload, coarse-grained locking is appropriate and safer.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: Shared between threads

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}
```java
// Paste your implementation here
```

**Justification**:Prevent lost updates 

---

### Critical Section #2: Execution Log

**What resource**: The shared resource is the executionLog which is an ArrayList<String> used by all threads to store execution messages.

**Why it needs protection**: ArrayList is not thread-safe. If multiple threads attempt to add elements simultaneously:
The internal array may resize concurrently.
Data may become corrupted.
A ConcurrentModificationException may occur.

**Synchronization mechanism used**: ReentrantLock was used to guarantee mutual exclusion when modifying the list.

**Code snippet**:
public static void logExecution(String message) {
    lock.lock();
    try {
        executionLog.add(message);
    } finally {
        lock.unlock();
    }
}
```java
// Paste your implementation here
```

**Justification**: Using a lock ensures that only one thread can modify the list at a time, preventing data corruption and runtime exceptions.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: To simulate a single CPU core.
In real operating systems, only one process can execute on a CPU core at a time.

**Number of permits and why**: Semaphore(1) → binary semaphore
This guarantees only one process executes at a time.

**Where implemented**: 
Inside:

run() method
runToCompletion() method

**Code snippet**:
SharedResources.cpuSemaphore.acquire();
try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
Every run completed successfully.
All processes finished execution and statistics were printed correctly each time.

(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)
Without synchronization, the following race conditions could occur:

Incorrect context switch count due to lost updates.
Incorrect waiting time accumulation.
Corrupted execution log due to concurrent ArrayList access.
Multiple processes executing simultaneously (unrealistic CPU behavior).

**Conclusion**: Synchronization ensures consistent and correct results across all executions.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: The program was executed multiple times while many threads were writing to executionLog

**Results**: No exceptions occurred during any run.

**What this proves**: The ReentrantLock successfully protected the shared ArrayList.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
Number of completed processes equals total processes.
Waiting time is positive and reasonable.
Execution log contains multiple entries.

**Actual values**: 
All statistics were printed correctly at the end of execution.

**Analysis**: This confirms correct synchronization and scheduler behavior.

---

### Test 4: Different Scenarios
**Scenario tested**: Running the scheduler multiple times with different randomly generated processes.

**Purpose**: To verify scheduler stability under varying workloads.

**Results**: Scheduler worked correctly in all scenarios.

**What I learned**: Synchronization guarantees reliability regardless of workload size or timing.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:
I learned how race conditions occur in multithreaded systems and how to prevent them using locks and semaphores. I also learned how to avoid deadlocks and ensure thread-safe programming.
[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking transactions

**Example 2**: Operating system process scheduling

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]
If I had to explain synchronization to someone who just finished Assignment 1, I would use a simple real-life analogy.

Imagine a single bathroom in a house shared by many people. If everyone tries to enter at the same time, chaos happens. Synchronization is like putting a lock on the door so only one person can use it at a time.

In this assignment:

The CPU is the bathroom.
The processes are the people.
The Semaphore is the key to the bathroom.
The ReentrantLock is the lock protecting shared notebooks (shared data).

Without synchronization, everyone would try to write in the notebook at the same time and the notes would become messy and incorrect. Synchronization ensures that threads take turns safely and the final results remain correct.

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/sarah9510is/OS-Assignment3-sarah-almutawa.git

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
