# Assignment 3 - Complete Documentation

**Student Name**: [mohra saad altamimi]  
**Student ID**: [445052020]  
**Date Submitted**: [Submission Date]

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

### Entry 1 - [May 2, 2026, 2:00 PM]
**What I implemented**: 
I began by looking for shared resources like contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog in the given code.
**Challenges encountered**: 
recognizing potential locations for race situations in a multi-threaded context.
**How I solved it**: 
I looked over Operating System ideas (10th Edition) synchronization ideas and identified important passages.
**Testing approach**: 
Before synchronization, I ran the software several times and noticed inconsistent behavior.
**Time spent**: 
2 hours
---

### Entry 2 - [May 2, 2026, 7:00 PM]
**What I implemented**: 
To safeguard shared counters, I created ReentrantLock.
**Challenges encountered**: 
ensuring that locks are always released in order to prevent deadlocks.
**How I solved it**: 
I surrounded every crucial area with try-finally blocks.
**Testing approach**: 
tested counter values following several iterations.
**Time spent**: 
2.5 hours
---

### Entry 3 - [May 3, 2026 3:00 PM]
**What I implemented**: 
I used a different lock to implement synchronization for executionLog.
**Challenges encountered**: 
preventing ConcurrentModificationException when an ArrayList is accessed by many threads.
**How I solved it**: 
used a special lock for the list called logLock.
**Testing approach**: 
checked logs to make sure there were no crashes.
**Time spent**: 
2 hours
---

### Entry 4 - [May 3, 2026, 6:00 PM]
**What I implemented**: 
To regulate CPU access, I included a semaphore.
**Challenges encountered**: 
knowing how to restrict the execution of threads.
**How I solved it**: 
utilized a single permit binary semaphore.
**Testing approach**: 
made sure that just one process runs concurrently.
**Time spent**: 
2 hours
---

### Entry 5 - [May 3, 2026 9:00 PM]
**What I implemented**: 
final examination and confirmation.
**Challenges encountered**: 
maintaining uniformity throughout several runs.
**How I solved it**: 
repeated execution and output verification.
**Testing approach**: 
ran the software five times.
**Time spent**: 
1.5 hours
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

The contextSwitchCount variable has one race condition. This shared variable is incremented concurrently by multiple threads, which could result in lost changes because of non-atomic operations. Another race condition in executionLog is the addition of elements to an ArrayList by several threads, which is not thread-safe. Inconsistent logs or even runtime errors like ConcurrentModificationException could arise from this. The final values of counters and logs would be erroneous and unpredictable in the absence of synchronization.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

Only one thread can access a crucial area at a time thanks to mutual exclusion provided by a ReentrantLock. It was employed in this assignment to safeguard the execution log and shared counters. In contrast, a semaphore regulates access to a restricted set of resources. To simulate CPU access and make sure that only one process runs at a time, I utilized a binary semaphore (with one permit). Semaphores are used to manage resources, whereas locks are intended to secure data.
---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

When two or more threads wait endlessly for resources owned by one another, this is known as deadlock. Using try-finally blocks to guarantee that locks are always released is one preventative strategy. Keeping a constant lock acquisition order is another strategy. By constantly releasing locks and semaphores in the finally block, I was able to avoid deadlocks in this code and make sure that no thread kept a resource permanently.
---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

To safeguard all three counters, I employed a single lock (coarse-grained locking). This method avoids complexity and streamlines the design. However, because the counters are independent, fine-grained locking—separate locks for each counter—might offer greater concurrency. Performance and simplicity are traded off. I gave simplicity and accuracy first priority in this project, which is appropriate for a classroom setting.
---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
They are updated simultaneously and shared by several threads.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
 counterLock.lock();
        try {
            contextSwitchCount++;
        } finally {
            // Task 1: Always release lock in finally block
            counterLock.unlock();
    }

**Justification**: 
Ensures mutual exclusion and prevents race conditions.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
It is not thread-safe to use an ArrayList.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
logLock.lock();
        try {
            executionLog.add(message);
        } finally {
            // Task 2: Always release lock in finally block
            logLock.unlock();
        }
```

**Justification**: 
avoids problems with concurrent modification.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To control access to CPU.
**Number of permits and why**: 
1 permit to allow only one process at a time.
**Where implemented**: 
In run() method.
**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
...
SharedResources.cpuSemaphore.release();

**Effect on program behavior**: 

Ensures serialized execution of processes.

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

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
