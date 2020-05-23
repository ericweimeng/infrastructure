# Concurrency

### Start a thread:

* Call threading.thread\(\) then start\(\), 
  * threading.thread\(\), this is the thread object
* Override run\(\)

### Quit a thread

* Executed all code in the target function
* Encountered Error/Exception

### Daemon thread

* a [`daemon`](https://en.wikipedia.org/wiki/Daemon_%28computing%29) is a process that runs in the background.

## Program vs Process vs Thread

This lesson discusses the differences between a program, process, and a thread. Also included is an psuedo code example of a thread-unsafe program.

#### Program vs Process vs Thread

**Program**

A program is a set of instructions and associated data that resides on the disk and is loaded by the operating system to perform a task. An executable file or a python script file are examples of programs. In order to run a program, the operating system's kernel is first asked to create a new process, which is an environment in which a program is executed.

**Process**

A process is a program in execution. A process is an execution environment that consists of instructions, user-data, and system-data segments, as well as lots of other resources such as CPU, memory, address-space, disk and network I/O acquired at runtime. A program can have several copies of it running at the same time but a process necessarily belongs to only one program.

**Thread**

A thread is the smallest unit of execution in a process which simply executes instructions serially. A process can have multiple threads running as part of it. Usually, there would be some state associated with the process that is shared among all the threads and in turn each thread would have some state private to itself.

The globally shared state amongst the threads of a process is visible and accessible to all the threads, and special attention needs to be paid when any thread tries to read or write to this global shared state. There are several constructs offered by various programming languages to guard and discipline the access to this global state, which we will go into further detail in upcoming lessons.

**Notes**

Note that a "program" and a "process" are often used interchangeably, though most of the time the intent is to refer to a process.

There's also the concept of "multiprocessing" systems, where multiple processes get scheduled on more than one CPU. Usually, this requires hardware support, where a single system comes with multiple cores or the execution takes place in a cluster of machines. Processes don't share any resources amongst themselves whereas threads of a process can share the resources allocated to that particular process, including memory address space. However, languages do provide facilities to enable inter-process communication.![widget](https://www.educative.io/api/collection/5307417243942912/5668546535227392/page/6532373392916480/image/6435356721283072.png)

**Counter Program**

Below is an example highlighting how multi-threading necessitates caution when accessing shared data amongst threads. Incorrect synchronization between threads can lead to wildly varying program outputs depending on the order in which threads get executed.

Consider the snippet of code below:

```text
1. counter = 0;
2.
3. def increment_counter():
4.     counter += 1
```

The increment on **line 4** is likely to be decompiled into the following steps on a computer:

* Read the value of the variable counter from the register where it is stored
* Add one to the value just read
* Store the newly computed value back to the register

The innocuous looking statement on **line 4** is really a three-step process!

Now imagine if we have two threads trying to execute the same function `increment_counter()`. One of the ways the execution of the two threads can take place is as follows:

Let's call one thread as **T1** and the other as **T2**. Say the counter value is equal to 7.

1. **T1** is currently scheduled on the CPU and enters the function. It performs step A, i.e. reads the value of the variable from the register, which is 7.
2. The operating system decides to context switch **T1** and bring in **T2**.
3. **T2** gets scheduled and luckily gets to complete all the three steps **A**, **B** and **C**, before getting switched out for **T1**. It reads the value 7, adds one to it and stores 8 back.
4. **T1** comes back, and since its state was saved by the operating system, it still has the stale value of 7 that it read before being context-switched. It doesn't know that the value of the variable has been updated behind its back and, unfortunately, thinking the value is still 7, adds one to it and overwrites the existing 8 with its own computed 8. If the threads executed serially the final value would have been 9.

These problems should be apparent to the astute reader. Without properly guarding access to mutable variables or data-structures, threads can cause hard-to-find to bugs.

Since the execution of the threads can't be predicted and is entirely up to the operating system, we can't make any assumptions about the order in which threads get scheduled and executed.

## Concurrency vs Parallelism

This lesson clarifies the common misunderstandings and confusions around concurrency and parallelism.

#### Concurrency vs Parallelism

Concurrency and Parallelism are often confused to refer to the ability of a system to run multiple distinct programs at the same time. Though the two terms are somewhat related they mean very different things. To clarify the concept, we'll borrow a juggler from a circus to use as an analogy. Consider the juggler to be a machine and the balls he juggles as processes.

**Serial Execution**

When programs are serially executed, they are scheduled one at a time on the CPU. Once a task gets completed, the next one gets a chance to run. Each task is run from beginning to end without interruption. The analogy for serial execution is a circus juggler who can only juggle one ball at a time. Definitely not very entertaining!

**Concurrency**

A concurrent program is one that can be decomposed into constituent parts and each part can be executed out of order or in partial order without affecting the final outcome. A system capable of running several distinct programs or more than one independent unit of the same program in overlapping time intervals is called a concurrent system. The execution of two programs or units of the same program may not happen simultaneously.

A concurrent system can have two programs in progress at the same time where progress doesn't imply execution. One program can be suspended while the other executes. Both programs are able to make progress as their execution is interleaved. In concurrent systems, the goal is to maximize throughput and minimize latency. For example, a browser running on a single core machine has to be responsive to user clicks but also be able to render HTML on screen as quickly as possible. Concurrent systems achieve lower latency and higher throughput when programs running on the system require frequent network or disk I/O.

The classic example of a concurrent system is that of an operating system running on a single core machine. Such an operating system is concurrent but not parallel. It can only process one task at any given point, but all the tasks being managed by the operating system appear to make progress because the operating system is designed for concurrency. Each task gets a slice of CPU time to execute and move forward.

Going back to our circus analogy, a concurrent juggler is one who can juggle several balls at the same time. However, at any one point in time, he can only have a single ball in his hand while the rest are in flight. Each ball gets a time slice during which it lands in the juggler's hand and then is thrown back up. A concurrent system is in a similar sense juggling several processes at the same time.

**Parallelism**

A parallel system is one which necessarily has the ability to execute multiple programs **at the same time.** Usually, this capability is aided by hardware in the form of multicore processors on individual machines or as computing clusters where several machines are hooked up to solve independent pieces of a problem simultaneously. Remember an individual problem has to be concurrent in nature, that is, portions of it can be worked on independently without affecting the final outcome before it can be executed in parallel.

In parallel systems, the emphasis is on increasing throughput and optimizing usage of hardware resources. The goal is to extract out as much computation speedup as possible. Example problems include matrix multiplication, 3D rendering, data analysis, and particle simulation.

Revisiting our juggler analogy, a parallel system would map to at least two or more jugglers juggling one or more balls. In the case of an operating system, if it runs on a machine with say four CPUs, then the operating system can execute four tasks at the same time, making execution parallel. Either a single \(large\) problem can be executed in parallel or distinct programs can be executed in parallel on a system supporting parallel execution.

**Concurrency vs Parallelism**

From the above discussion it should be apparent that a concurrent system need not be parallel, whereas a parallel system is indeed concurrent. Additionally, a system can be both concurrent and parallel, e.g. a multitasking operating system running on a multicore machine.

Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once. Last but not the least, you'll find literature describing concurrency as a property of a program or a system whereas parallelism as a runtime behaviour of executing multiple tasks.

We end the lesson with an analogy, frequently quoted in online literature, of customers waiting in two queues to buy coffee. Single-processor concurrency is akin to alternatively serving customers from the two queues but with a single coffee machine, while parallelism is similar to serving each customer queue with a dedicated coffee machine.

![widget](https://www.educative.io/api/collection/5307417243942912/5668546535227392/page/6314227553796096/image/6636670789091328.png)

## Cooperative vs Preemptive Multitasking

This lesson details the differences between the two common models of multitasking.

#### Cooperative vs Preemptive Multitasking

A system can achieve concurrency by employing one of the following multitasking models:

* Preemptive Multitasking
* Cooperative Multitasking

**Preemptive Multitasking**

In preemptive multitasking, the operating system preempts a program to allow another waiting task to run on the CPU. Programs or threads can't decide how long or when they can use the CPU. The operating system’s scheduler decides which thread or program gets to use the CPU next and for how much time. Furthermore, scheduling of programs or threads on the CPU isn’t predictable. A thread or program, once taken off of the CPU by the scheduler, can't determine when it will get on the CPU next. As a consequence, if a malicious program initiates an infinite loop, it only hurts itself without affecting other programs or threads. Lastly, the programmer isn't burdened to decide when to give control back to the CPU in code.

**Cooperative Multitasking**

Cooperative Multitasking involves well-behaved programs voluntarily giving up control back to the scheduler so that another program can run. A program or thread may give up control after a period of time has expired or if it becomes idle or logically blocked. The operating system’s scheduler has no say in how long a program or thread runs for. A malicious program can bring the entire system to a halt by busy-waiting or running an infinite loop and not giving up control. The process scheduler for an operating system implementing cooperative multitasking is called a cooperative scheduler. As the name implies, the participating programs or threads are required to cooperate to make the scheduling scheme work.

**Cooperative vs Preemptive**

Early versions of both Windows and Mac OS used cooperative multitasking. Later on, preemptive multitasking was introduced in Windows NT 3.1 and in Mac OS X. However, preemptive multitasking has always been a core feature of Unix based systems.

## Throughput vs Latency

This lessons discusses throughput and latency in the context of concurrent systems.

#### Throughput vs Latency

**Throughput**

Throughput is defined as the rate of doing work or how much work gets done per unit of time. If you are an Instagram user, you could define throughput as the number of images downloaded by your phone or browser per unit of time.

**Latency**

Latency is defined as the time required to complete a task or produce a result. Latency is also referred to as response time. The time it takes for a web browser to download Instagram images from the internet is the latency for downloading the images.

**Throughput vs Latency**

The two terms are more frequently used when describing networking links and have more precise meanings in that domain. In the context of concurrency, throughput can be thought of as time taken to execute a program or computation. For instance, imagine a program that is given hundreds of files containing integers and asked to sum up all the numbers. Since addition is commutative, each file can be worked on in parallel. In a single-threaded environment, each file will be sequentially processed, but in a concurrent system, several threads can work in parallel on distinct files. Of course, there will be some overhead to manage the state, including already processed files. However, such a program will complete the task much faster than a single thread. The performance difference will become more and more apparent as the number of input files increases. The throughput in this example can be defined as the number of files processed by the program in a minute. Latency can be defined as the total time taken to completely process all the files. As you observe, in a multithreaded implementation throughput will go up and latency will go down. More work gets done in less amount of time. In general, the two have an inverse relationship.

## Synchronous vs Asynchronous

This lesson discusses the differences between asynchronous and synchronous programming which are often talked about in the context of concurrency.

#### Synchronous vs Asynchronous

**Synchronous**

Synchronous execution refers to line-by-line execution of code. If a function is invoked, the program execution waits until the function call is completed. Synchronous execution blocks at each method call before proceeding to the next line of code. A program executes in the same sequence as the code in the source code file. Synchronous execution is synonymous with serial execution.

**Asynchronous**

Asynchronous \(or async\) execution refers to execution that doesn't block when invoking subroutines. Or, if you prefer the more fancy Wikipedia definition: Asynchronous programming is a means of parallel programming in which a unit of work runs separately from the main application thread and notifies the calling thread of its completion, failure or progress. An asynchronous program doesn’t wait for a task to complete before moving on to the next task.

In contrast to synchronous execution, asynchronous execution doesn't necessarily execute code line by line, that is, instructions may not run in the sequence that they appear in the code. Async execution can invoke a method and move on to the next line of code without waiting for the invoked function to complete or receive its result. Usually, such methods return an entity sometimes called **future** or **promise** that is the representation of an in-progress computation. The program can query for the status of the computation via the returned future or promise and retrieve the result once completed. Another pattern is to pass a callback function to the asynchronous function call, which is invoked with the results when the asynchronous function is done processing. Asynchronous programming is an excellent choice for applications that do extensive network or disk I/O and spend most of their time waiting. As an example, Javascript enables concurrency using AJAX library's asynchronous method calls. In non-threaded environments, asynchronous programming provides an alternative to threads in order to achieve concurrency and fall under the cooperative multitasking model.

## I/O Bound vs CPU Bound

In this lesson, we delve into the characteristics of programs with different resource-use profiles and how that can affect program design choices.

#### I/O Bound vs CPU Bound

We write programs to solve problems. Programs utilize various resources of the computer systems on which they run. For instance a program running on your machine will broadly require:

* CPU Time
* Memory
* Networking Resources
* Disk Storage

Depending on what a program does, it can require heavier use of one or more resources. For instance, a program that loads gigabytes of data from storage into main memory would hog the main memory of the machine it runs on. Another can be writing several gigabytes to permanent storage, requiring abnormally high disk I/O.

**CPU Bound**

Programs which are compute-intensive, i.e. where the program execution requires very high utilization of the CPU \(close to 100%\), are called CPU bound programs. Such programs primarily depend upon improving CPU speed to decrease program completion time. This could include programs such as data crunching, image processing, matrix multiplication, etc.

If a CPU bound program is provided a more powerful CPU it can potentially complete faster. Eventually, there is a limit on how powerful a single CPU can be. At this point, the recourse is to harness the computing power of multiple CPUs and structure your program code in a way that can take advantage of the multiple CPU units available. Say we are trying to sum up the first 1 million natural numbers. A single-threaded program would sum in a single loop from 1 to 1000000. To cut down on execution time, we can create two threads and divide the range into two halves. The first thread sums up the numbers from 1 to 500000 and the second sums up the numbers from 500001 to 1000000. If there are two processors available on the machine, then each thread can independently run on a single CPU in parallel. In the end, we sum up the results from the two threads to get the final result. Theoretically, the multithreaded program should finish in half the time that it takes for the single-threaded program. However, there will be a slight overhead of creating the two threads and merging the results from the two threads.

Multithreaded programs can improve performance in cases where the problem lends itself to being divided into smaller pieces that different threads can work on independently. However, this may not always be true.

**I/O Bound**

I/O bound programs are the opposite of CPU bound programs. Such programs spend most of their time waiting for input or output operations to complete while the CPU sits idle. I/O operations can consist of operations that write or read from main memory or network interfaces. Because the CPU and main memory are physically separate, a data bus exists between the two to transfer bits to and fro. Similarly, data needs to be moved between network interfaces and CPU/memory. Even though the physical distances are tiny, the time taken to move the data across is long enough for several thousand CPU cycles to go to waste. This is why I/O bound programs would show relatively lower CPU utilization than CPU bound programs.

**Notes**

Both types of programs can benefit from concurrent architectures. If a program is CPU bound, we can increase the number of processors and structure our program to spawn multiple threads that run individually on a dedicated or shared CPU. For I/O bound programs, it makes sense to have a thread give up CPU control if it is waiting for an I/O operation to complete so that another thread can get scheduled on the CPU and utilize CPU cycles. Different programming languages come with varying support for multithreading. For instance, JavaScript is single-threaded, Java provides full-blown multithreading and Python is sort of multithreaded as you'll learn in later chapters. However, all three languages support asynchronous programming models which is another way for programs to be concurrent \(but not parallel\).

For completeness we should mention that there are also memory-bound programs that depend on the amount of memory available to speed up execution. Python manages memory on behalf of the user.

## Thread Safety

This lessons discusses the concept of thread safety.

#### Thread Safety

The primary motivation behind using multiple threads is improving program performance that may be measured with metrics such as throughput, responsiveness, latency, etc. Whenever threads are introduced in a program, the shared state amongst the threads becomes vulnerable to corruption. If a class or a program has immutable state then the class is necessarily thread-safe. Similarly, the shared state in an application where the same thread mutates the state using an operation that translates into an atomic bytecode instruction can be safely read by multiple reader threads. In contrast, a sole writer thread mutating the shared state using several atomic bytecode instructions isn't a thread-safe scenario for reader threads. Most multi-threaded setups require caution when interacting with shared state. As a corollary, the composition of two thread-safe classes doesn't guarantee thread-safety.

**Atomicity**

Consider the below snippet:

```text
count = 0
 
def increment():
    global count
    count += 1
```

The above code will work flawlessly when it is executed by a single thread. However, if there are two or more threads involved, things get tricky. The key to realize is that the statement `count += 1` isn't atomic. A thread can't increment the variable atomically, i.e. there doesn't exist a single bytecode instruction that can increment the count variable. Let's examine the bytecode generated for our snippet above.

```python
import dis

count = 0

def increment():
    global count
    count += 1

# prints the bytecode
dis.dis(increment)
```

**Generated Byte Code**

**7 0 LOAD\_GLOBAL 0 \(count\)  
3 LOAD\_CONST 1 \(1\)  
6 INPLACE\_ADD  
7 STORE\_GLOBAL 0 \(count\)  
10 LOAD\_CONST 0 \(None\)  
13 RETURN\_VALUE**  


The seemingly single line statement expands into multiple bytecode instructions. When two threads invoke the `increment()` method it is possible that the first thread is switched out by the Python interpreter just before the third **INPLACE\_ADD** instruction is executed. Now the second thread comes along and executes all the six bytecode instructions in one go. When the first thread is rescheduled by the interpreter, it executes the third line but the value the thread holds is stale causing it to incorrectly update the `count` variable.

Programming languages provide constructs such as mutexes and locks to help developers guard sections of code that must be executed sequentially by multiple threads. Guarding shared data is one aspect of multi-threaded programs. The other aspect is coordination and cooperation amongst threads. Again, languages provide mechanisms to facilitate threads to work cooperatively towards a common goal. These include semaphores, barriers etc.

**Thread Unsafe Class**

Take a minute to go through the following program. It increments an object of class `Counter` using 5 threads. Each thread increments the object a hundred thousand times. The final value of the counter should be half a million \(500,000\). If you run the program enough times, you'll sometimes get the correct summation, and at others, you'll get an incorrect value.

```python
from threading import Thread
import sys

class Counter:

    def __init__(self):
        self.count = 0

    def increment(self):
        for _ in range(100000):
            self.count += 1


if __name__ == "__main__":
    
    # Sets the thread switch interval
    sys.setswitchinterval(0.005)

    numThreads = 5
    threads = [0] * numThreads
    counter = Counter()

    for i in range(0, numThreads):
        threads[i] = Thread(target=counter.increment)

    for i in range(0, numThreads):
        threads[i].start()

    for i in range(0, numThreads):
        threads[i].join()

    if counter.count != 500000:
        print(" count = {0}".format(counter.count), flush=True)
    else:
        print(" count = 50,000 - Try re-running the program.")


```

**Fixing Thread Unsafe Class**

We fix the above example using the equivalent of a mutex in Python called a `Lock`. For now, don't worry about how the example below works, but observe how the count always sums up to half a million.

```python
from threading import Thread
from threading import Lock
import sys

class Counter:

    def __init__(self):
        self.count = 0
        self.lock = Lock()

    def increment(self):
        for _ in range(100000):
            self.lock.acquire()
            self.count += 1
            self.lock.release()


if __name__ == "__main__":
    
    # Sets the thread switch interval
    sys.setswitchinterval(0.005)

    numThreads = 5
    threads = [0] * numThreads
    counter = Counter()

    for i in range(0, numThreads):
        threads[i] = Thread(target=counter.increment)

    for i in range(0, numThreads):
        threads[i].start()

    for i in range(0, numThreads):
        threads[i].join()

    if counter.count != 500000:
        print(" If this line ever gets printed, " + \
        "the author is a complete idiot and " + \
        "you should return the course for a full refund!")
    else:
        print(" count = {0}".format(counter.count))
```

## Critical Section & Race Conditions

This lesson exhibits how incorrect synchronization in a critical section can lead to race conditions and buggy code. The concepts of critical section and race condition are explained in depth. Also included is an executable example of a race condition.

#### Critical Section & Race Conditions

A program is a set of instructions being executed and multiple threads of a program can be executing different sections of the program code. However, caution should be exercised when threads of the same process attempt to simultaneously execute the same portion of code.

**Critical Section**

Critical section is any piece of code that has the possibility of being executed concurrently by more than one thread of the application and exposes any shared data or resources used by the application for access.

**Race Condition**

Race conditions happen when threads run through critical sections without thread synchronization. The threads "race" through the critical section to write or read shared resources and depending on the order in which threads finish the "race", the program output changes. In a race condition, threads access shared resources or program variables that might be worked on by other threads at the same time causing the application data to be inconsistent.

As an example, consider a thread that tests for a state/condition, called a predicate, and then takes subsequent action based on that condition. This sequence is called **test-then-act**. The pitfall here is that the state can be mutated by the second thread just after the test by the first thread and before the first thread takes action based on the test. A different thread changes the predicate in between the **test and act**. In this case, action by the first thread is not justified since the predicate doesn't hold when the action is executed.

**Example Thread Race**

Consider the snippet below. We have two threads working on the same variable `rand_int`. The modifier thread perpetually updates the value of `rand_int` in a loop while the printer thread prints the value of `rand_int` only if `rand_int` is divisible by 5. If you let this program run, you'll notice some values get printed even though they aren't divisible by 5 demonstrating a thread-unsafe version of **test-then-act**.

```python
from threading import *
import random
import time

rand_int = 0


def updater():
    global rand_int
    while 1:
        rand_int = random.randint(1, 9)


def printer():
    global rand_int
    while 1:

        # test
        if rand_int % 5 == 0:
            if rand_int % 5 != 0:
                # and act
                print(rand_int)


if __name__ == "__main__":
    Thread(target=updater, daemon=True).start()
    Thread(target=printer, daemon=True).start()

    # Let the simulation run for 5 seconds
    time.sleep(5)

```

## Deadlock, Liveness & Reentrant Locks

We discuss important concurrency concepts such as deadlock, liveness, live-lock, starvation and reentrant locks in depth. Also included are executable code examples for illustrating these concepts.

#### Deadlock and Liveness

Logical follies committed in multithreaded code, while trying to avoid race conditions and guarding critical sections, can lead to a host of subtle and hard to find bugs and side-effects. Some of these incorrect usage patterns have their specific names and are discussed below.

**Deadlock**

Deadlocks occur when two or more threads aren't able to make any progress because the resource required by the first thread is held by the second and the resource required by the second thread is held by the first.

**Liveness**

Ability of a program or an application to execute in a timely manner is called liveness. If a program experiences a deadlock then it's not exhibiting liveness.

**Live-Lock**

A live-lock occurs when two threads continuously react in response to the actions by the other thread without making any real progress. The best analogy is to think of two persons trying to cross each other in a hallway. John moves to the left to let Arun pass, and Arun moves to his right to let John pass. Both block each other now. John sees he's blocking Arun again and moves to his right and Arun moves to his left seeing he's blocking John. They never cross each other and keep blocking each other. This scenario is an example of a live-lock. A process seems to be running and not deadlocked but in reality, isn't making any progress.

**Starvation**

Other than a deadlock, an application thread can also experience starvation when it never gets CPU time or access to shared resources. Other **greedy** threads continuously hog shared system resources not letting the starving thread make any progress.

**Deadlock Example**

Consider the pseudocode below:

```python
void increment(){
 
  acquire MUTEX_A
  acquire MUTEX_B
    // do work here
  release MUTEX_B
  release MUTEX_A
    
}
 
 
void decrement(){
  
  acquire MUTEX_B
  acquire MUTEX_A
    // do work here
  release MUTEX_A
  release MUTEX_B
    
}
```

The above code can potentially result in a deadlock. Note that deadlock may not always happen, but for certain execution sequences, deadlock can occur. Consider the execution sequence below that ends up in a deadlock:

```text
T1 enters function increment
 
T1 acquires MUTEX_A
 
T1 gets context switched by the operating system
 
T2 enters function decrement
 
T2 acquires MUTEX_B
 
both threads are blocked now
```

Thread **T2** can't make progress as it requires **MUTEX\_A** which is being held by **T1**. Now when **T1** wakes up, it can't make progress as it requires **MUTEX\_B** and that is being held up by **T2**. This is a classic text-book example of a deadlock.

You can come back to the examples presented below as they require an understanding of the `Lock` and `RLock` classes. Or you can just run the examples and observe the output for now to get a high-level overview of the concepts we discussed in this lesson.

If you run the code snippet below, you'll see that the statements for acquiring locks: **lock1** and **lock2** print out but there's no progress after that and the execution times out. In this scenario, the deadlock occurs because the locks are being acquired in a nested fashion.

```python
from threading import *
import time

def thread_A(lock1, lock2):
    lock1.acquire()
    print("{0} acquired lock1".format(current_thread().getName()))
    time.sleep(1)
    lock2.acquire()
    print("{0} acquired both locks".format(current_thread().getName()))


def thread_B(lock1, lock2):
    lock2.acquire()
    print("{0} acquired lock2".format(current_thread().getName()))
    time.sleep(1)
    lock1.acquire()
    print("{0} acquired both locks".format(current_thread().getName()))


if __name__ == "__main__":

    lock1 = Lock()
    lock2 = Lock()

    Thread(target=thread_A, args=(lock1, lock2)).start()
    Thread(target=thread_B, args=(lock1, lock2)).start()
```

**Reentrant Lock**

Reentrant locks allow for re-locking or re-entering of a synchronization lock. If a synchronization primitive doesn't allow reacquisition of itself by a thread that has already acquired it, then such a thread would block as soon as it attempts to reacquire the primitive a second time.

This is best explained with an example. Consider the NonReentrant class below, where we are using an ordinary `Lock` object that results in a deadlock. The execution of the code widget would time out when it is run.

```python
from threading import *


if __name__ == "__main__":
  ordinary_lock = Lock()

  ordinary_lock.acquire()
  ordinary_lock.acquire()
  
  print("{0} exiting".format(current_thread().getName()))

  ordinary_lock.release()
  ordinary_lock.release()
```

The above example works when we change the lock from an instance of `Lock` to `RLock`. The latter allows reentry by a thread already holding the lock and is thus called a **reentrant lock**.

```python
from threading import *


if __name__ == "__main__":
  ordinary_lock = RLock()
  ordinary_lock.acquire()
  ordinary_lock.acquire()
  
  print("{0} exiting".format(current_thread().getName()))

  ordinary_lock.release()
  ordinary_lock.release()

```



## Mutex vs Semaphore

The concept of and the difference between a mutex and a semaphore will draw befuddled expressions on most developers' faces. In this lesson, we discuss the differences between the two most fundamental concurrency constructs offered by almost all language frameworks. Difference between a mutex and a semaphore makes a pet interview question for senior engineering positions!

#### Mutex vs Semaphore

Having laid the foundation of concurrent programming concepts and their associated issues, we'll now discuss the all-important mechanisms of locking and signaling in multi-threaded applications and the differences between these constructs.

**Mutex**

Mutex as the name hints implies mutual exclusion. A mutex is used to guard shared data such as a linked-list, an array, or any primitive type. A mutex allows only a single thread to access a resource or critical section.

Once a thread acquires a mutex, all other threads attempting to acquire the same mutex are blocked until the first thread releases the mutex. Once released, most implementations arbitrarily choose one of the waiting threads to acquire the mutex and make progress.

**Semaphore**

Semaphore, on the other hand, is used for limiting access to a collection of resources. Think of semaphore as having a limited number of permits to give out. If a semaphore has given out all the permits it has, then any new thread that comes along requesting a permit will be blocked till an earlier thread with a permit returns it to the semaphore. A typical example would be a pool of database connections that can be handed out to requesting threads. Say there are ten available connections but 50 requesting threads. In such a scenario, a semaphore can only give out ten permits or connections at any given point.

A semaphore with a single permit is called a **binary semaphore** and is often thought of as equivalent to a mutex, which isn't completely correct as we'll shortly explain. Semaphores can also be used for signaling among threads. This is an important distinction as it allows threads to cooperatively work towards completing a task. A mutex, on the other hand, is strictly limited to serializing access to shared state among competing threads.

**Mutex Example**

The following illustration shows how two threads acquire and release a mutex one after the other to gain access to shared data. Mutex guarantees that the shared state isn't corrupted when competing threads work on it.![widget](https://www.educative.io/api/collection/5307417243942912/5668546535227392/page/4810492780478464/image/5727208351989760.png)

**When can a Semaphore masquerade as a Mutex?**

A semaphore can potentially act as a mutex if the permits it can give out is set to 1. However, the most important difference between the two is that in case of a mutex the same thread must call acquire and subsequent release on the mutex whereas in case of a binary semaphore, different threads can call acquire and release on the semaphore. The pthreads library documentation states this in the `pthread_mutex_unlock()` method's description.

{% hint style="info" %}
If a thread attempts to unlock a mutex that it has not locked or a mutex which is unlocked, undefined behavior results.
{% endhint %}

This leads us to the concept of **ownership**. A mutex is owned by the thread acquiring it till the point the owning-thread releases it, whereas for a semaphore there's no notion of ownership.

**Semaphore for Signaling**

Another distinction between a semaphore and a mutex is that semaphores can be used for signaling amongst threads. For example, in case of the classical **producer/consumer** problem the producer thread can signal the consumer thread by incrementing the semaphore count to indicate to the consumer thread to consume the freshly produced item. A mutex, in contrast, only guards access to shared data among competing threads by forcing threads to serialize their access to critical sections and shared data-structures.

Below is a pictorial representation of how a mutex works.

![widget](https://www.educative.io/api/collection/5307417243942912/5668546535227392/page/4810492780478464/image/6243387755724800.png)

Below is a depiction of how a semaphore works. The semaphore initially has two permits and allows at most two threads to enter the critical section or access protected resources

![widget](https://www.educative.io/api/collection/5307417243942912/5668546535227392/page/4810492780478464/image/5327465209659392.png)

**Summary**

1. Mutex implies mutual exclusion and is used to serialize access to critical sections whereas semaphore can potentially be used as a mutex, but it can also be used for cooperation and signaling amongst threads. Semaphore also solves the issue of **missed signals**.
2. Mutex is **owned** by a thread, whereas a semaphore has no concept of ownership.
3. Mutex, if locked, must necessarily be unlocked by the same thread. A semaphore can be acted upon by different threads. This is true even if the semaphore has a permit of one
4. Think of semaphore as analogous to a car rental service such as Hertz. Each outlet has a certain number of cars it can rent out to customers. It can rent several cars to several customers at the same time but if all the cars are rented out, then any new customers need to be put on a waitlist till one of the rented cars is returned. In contrast, think of a mutex like a lone runway on a remote airport. Only a single jet can land or take-off from the runway at any given point. No other jet can use the runway simultaneously with the first aircraft.

## Mutex vs Monitor

Learn what a monitor is and how it is different than a mutex. Monitors are advanced concurrency constructs and specific to languages frameworks.

#### Mutex vs Monitor

Continuing our discussion from the previous section on locking and signaling mechanisms, we'll now pore over an advanced concept, the **monitor**. It is exposed as a concurrency construct by some programming language frameworks, including Python.

Concisely, a monitor is a mutex and then some. The additional composition of monitors consists of condition variables, which we shortly describe. Monitors are generally language-level constructs whereas mutex and semaphore are lower-level or OS-provided constructs.

To understand monitors, let's first see the problem they solve. Usually, in multi-threaded applications, a thread needs to wait for some program predicate to be true before it can proceed forward. Think about a producer/consumer application. If the producer hasn't produced anything the consumer can't consume anything. So the consumer must **wait on** a predicate that lets the consumer know that something has indeed been produced. What could be a crude way of accomplishing this? The consumer could repeatedly check in a loop for the predicate to be set to true. The pattern would resemble the pseudocode below:

`void` **`busy_wait_function()`** `{  
    // acquire mutex  
    while (predicate is false) {  
      // release mutex  
      // acquire mutex  
    }  
    // do something useful  
    // release mutex  
}`

Within the while loop we'll first release the mutex giving other threads a chance to acquire it, and set the loop predicate to true. Before we check the loop predicate again, we make sure we have acquired the mutex. This works but is an example of "spin waiting" which wastes a lot of CPU cycles. Let's see how condition variables solve the spin-waiting issue.

**Condition Variables**

Mutex provides mutual exclusion. However, at times mutual exclusion is not enough. We want to test for a predicate with a mutually exclusive lock so that no other thread can change the predicate when we test for it, but if we find the predicate to be false, we'd want to wait on a condition variable till the predicate's value is changed. This is the solution to spin waiting.

Conceptually, each condition variable exposes two methods `wait()` and `signal()`. The `wait()` method, when called on the condition variable, will cause the associated mutex to be atomically released and the calling thread would be placed in a **wait queue**. There could already be other threads in the **wait queue** that previously invoked `wait()` on the condition variable. Since the mutex is now released, it gives other threads a chance to change the predicate that will eventually let the thread that was just placed in the wait queue to make progress. As an example, say we have a consumer thread that checks for the size of the buffer, finds it empty and invokes `wait()` on a condition variable. The predicate in this example would be **the size of the buffer**.

Now imagine a producer places an item in the buffer. The predicate, the size of the buffer, just changed and the producer wants to let the consumer threads know that there is an item to be consumed. This producer thread would then invoke `signal()` on the condition variable. The `signal()` method, when called on a condition variable, causes one of the threads that has been placed in the **wait queue** to get ready for execution. Note that we didn't say the woken up thread starts executing. It just gets ready - and that could mean being placed in the ready queue. It is only after the producer thread which calls the `signal()` method has released the associated mutex that the thread in the ready queue starts executing. The thread in the ready queue must wait to acquire the mutex associated with the condition variable before it can start executing.

Let's see how this translates into code.  
`void` **`efficient_waiting_function()`** `{  
    mutex.acquire()  
    while (predicate == false) {  
      cond_var.wait()  
    }  
    // Do something useful  
    mutex.release()       
}  
  
void` **`change_predicate()`** `{  
    mutex.acquire()  
    set predicate = true  
    cond_var.signal()  
    mutex.release()  
}`  


Let's dry run the above code. Say **thread A** executes `efficient_waiting_function()` first and finds the loop predicate is false and enters the loop. Next, **thread A** executes the statement `cond_var.wait()` and is placed in a wait queue. At the same time **thread A** gives up the mutex. Now **thread B** comes along and executes `change_predicate()` method. Since the mutex was given up by **thread A**, **thread B** is able to acquire it and set the predicate to true. Next, it signals the condition variable `cond_var.signal()`. This step places **thread A** into the ready queue but **thread A** doesn't start executing until **thread B** has released the mutex.

Note that the order of signaling the condition variable and releasing the mutex can be interchanged, but generally, the preference is to signal first and then release the mutex. However, the ordering might have ramifications on thread scheduling depending on the threading implementation.

**Why the while Loop**

The wary reader would have noticed us using a while loop to test for the predicate. After all, the pseudocode could have been written as follows:  
`void` **`efficient_waiting_function()`** `{  
    mutex.acquire()`  
    **`if (predicate == false)`** `{  
      cond_var.wait()  
    }  
    // Do something useful  
    mutex.release()       
}`  


If the snippet is re-written in the above manner using an `if` clause instead of a `while` then, we need a guarantee that once the variable `cond_var` is signaled, the predicate can't be changed by any other thread and that the signaled thread becomes the owner of the monitor. This may not be true. For one, a different thread could get scheduled and change the predicate back to false before the signaled thread gets a chance to execute, therefore the signaled thread must again check the predicate once it acquires the monitor. Secondly, use of the loop is necessitated by design choices of monitors that we'll explore in the next section. Last but not the least, on POSIX systems, **spurious or fake wakeups** are possible \(also discussed in later chapters\) even though the condition variable has not been signaled and the predicate hasn't changed. The idiomatic and correct usage of a monitor dictates that the predicate always be tested for in a while loop.

**Monitor Explained**

After the above discussion, we now realize that a monitor is made up of a mutex and one or more condition variables. A single monitor can have multiple condition variables but not vice versa. Theoretically, another way to think about a monitor is to consider it as an entity having two queues or sets where threads can be placed. One is the **entry set** and the other is the **wait set**. When a thread A **enters** a monitor, it is placed into the **entry set**. If no other thread **owns** the monitor, which is equivalent of saying no thread is actively executing within the monitor section, then thread A will **acquire** the monitor and is said to own it too. Thread A will continue to execute within the monitor section till it **exits** the monitor or calls `wait()` on an associated condition variable and be placed into the wait set. While thread A **owns** the monitor, no other thread will be able to execute any of the critical sections protected by the monitor. New threads requesting ownership of the monitor get placed into the **entry set**.

Continuing with our hypothetical example, say another thread B comes along and gets placed in the **entry set** while thread A sits in the **wait set**. Since no other thread owns the monitor, thread B successfully acquires the monitor and continues execution. If thread B exits the monitor section without calling `notify()` on the condition variable, then thread A will remain waiting in the **wait set**. Thread B can also invoke `wait()` and be placed in the **wait set** along with thread A. This then would require a third thread to come along and call `notify()` on the condition variable on which both threads A and B are waiting. Note that only a single thread will be able to **own** the monitor at any given point and will have exclusive access to data structures or critical sections protected by the monitor.

Practically, in Python, a `Condition` object is a monitor which implicitly has a lock or can be passed one explicitly. You can think of a monitor as a mutex with a wait set. Monitors allow threads to exercise **mutual exclusion** as well as **cooperation** by allowing them to wait and signal on conditions.

A pictorial representation of the working of a monitor is shown below:

![widget](https://www.educative.io/api/collection/5307417243942912/5668546535227392/page/4656073355034624/image/5168917473394688.png)

## RLock

This lesson explains the reentrant lock in python's threading module.

#### Introduction

A reentrant lock is defined as a lock which can be reacquired by the same thread. A `RLock` object carries the notion of ownership. If a thread acquires a `RLock` object, it can chose to reacquire it as many times as possible. Consider the following snippet:

**Reentrant lock**

```text
# create a reentrant lock
rlock = RLock()
 
# acquire the lock twice
rlock.acquire()
rlock.acquire()
 
# release the lock twice
rlock.release()
rlock.release()
 
```

In contrast to `Lock`, the reentrant lock is acquired twice in the above snippet without blocking. Note that it is imperative to release the lock as many times as it is locked, otherwise the lock remains in locked state and any other threads attempting to acquire the lock get blocked. This is shown with an example below:

```python
from threading import RLock
from threading import Thread


def child_task():
    rlock.acquire()
    print("child task executing")
    rlock.release()


rlock = RLock()

rlock.acquire()
rlock.acquire()

rlock.release()

# UNCOMMENT THE FOLLOWING LINE TO MAKE THE
# PROGRAM EXIT NORMALLY.
# rlock.release()

thread = Thread(target=child_task)
thread.start()
thread.join()
```

If you uncomment **line 20** in the above code widget the child thread would be able to execute. We can have arbitrary nesting of acquiring and releasing of the reentrant lock. For instance, the following would work:

```text
rlock = RLock()
 
rlock.acquire()
rlock.acquire()
rlock.acquire()
rlock.release()
 
rlock.acquire()
rlock.release()
rlock.release()
rlock.release()
```

The nested acquire/release calls are tracked internally by recursion level which is incremented on every `acquire()` and decremented on every `release()` by the same thread. When the recursion level is zero, the reentrant lock is in unlocked state.

**Ownership**

As explained, each reentrant lock is owned by some thread when in the locked state. Only the owner thread is allowed to exercise a `release()` on the lock. If a thread different than the owner invokes `release()` a RuntimeError is thrown as shown in the example below:

**Releasing unowned reentrant lock**

```text
from threading import RLock
from threading import Thread
 
 
def perform_unlock():
    rlock.release()
    print("child task executing")
    rlock.release()
 
 
rlock = RLock()
 
# reentrant lock acquired by main thread
rlock.acquire()
 
# let's attempt to unlock using a child thread
thread = Thread(target=perform_unlock)
thread.start()
thread.join()
 
```

Recognize this isn't a problem with non-reentrant locks.

