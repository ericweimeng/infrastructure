# Threading Module

## Creating Threads

This lesson demonstrates how threads can be created in Python.

#### Creating Threads

We can create threads in Python using the `Thread` class. The constructor for the thread class appears below:

**Thread constructor**

```text
Thread(group=None, target=None, name=None, args=(), kwargs={}, *, daemon=None)
```

The `group` argument is reserved for future extensions. `target` is the code that the thread being created will execute. It can be any callable object. A callable object can be a function or an object of a class that has the `__call__` method. Next, we can pass the arguments to the target either as a tuple `args` or as a dictionary of key value pairs using `kwargs`. Finally, the constructor takes a boolean specifying if the thread being created should be treated as a daemon thread.

The below snippet creates a thread using the `Thread` class's constructor:

**Creating thread**

```python
from threading import Thread
from threading import current_thread
 
 
def thread_task(a, b, c, key1, key2):
    print("{0} received the arguments: {1} {2} {3} {4} {5}".format(current_thread().getName(), a, b, c, key1, key2))
 
 
myThread = Thread(group=None,  # reserved
                  target=thread_task,  # callable object
                  name="demoThread",  # name of thread
                  args=(1, 2, 3),  # arguments passed to the target
                  kwargs={'key1': 777,
                          'key2': 111},  # dictionary of keyword arguments
                  daemon=None  # set true to make the thread a daemon
                  )
 
myThread.start()  # start the thread
 
myThread.join()  # wait for the thread to complete
```

Once the thread object is created, we must start it by invoking the `start()` method. Once started, the thread will execute the target.

**Main Thread**

The astute reader would realize that there has to be another thread that is actually executing the code we wrote to create a new thread. This ab initio thread is called the **main thread**. Under normal conditions, the main thread is the thread from which the Python interpreter was started. The below image captures the above script in debug mode. Notice the debugger dropdown displays two threads:

* Main Thread
* demoThread

![widget](https://www.educative.io/api/collection/5307417243942912/5668546535227392/page/4916062241947648/image/6389748836859904.png)

## Subclassing Thread

This lesson demonstrates how we can subclass the Thread class to create threads.

#### Subclassing Thread

Another way to create threads is to subclass the `Thread` class. As mentioned earlier, the `threading` module is inspired from Java and Java offers a similar way of creating threads by subclassing. Consider the snippet below:

**Creating threads by subclassing Thread class**

```python
class MyTask(Thread):
 
    def __init__(self):
        Thread.__init__(self, name="subclassThread", args=(2, 3))
 
    def run(self):
        print("{0} is executing".format(current_thread().getName()))
 
```

The important caveats to remember when subclassing the `Thread` class are:

* We can only override the `run()` method and the constructor of the `Thread` class.
* `Thread.__init__()` must be invoked if the subclass choses to override the constructor.
* Note that the `args` or `kwargs` don't get passed to the `run` method.

```python
from threading import Thread
from threading import current_thread


class MyTask(Thread):

    def __init__(self):
        # The two args will not get passed to the overridden
        # run method.
        Thread.__init__(self, name="subclassThread", args=(2, 3))

    def run(self):
        print("{0} is executing".format(current_thread().getName()))


myTask = MyTask()

myTask.start()  # start the thread

myTask.join()  # wait for the thread to complete

print("{0} exiting".format(current_thread().getName()))
```

## Daemon Thread

This lesson describes how daemon threads exit as soon as the main program thread exits.

#### Daemon Thread

In Greek mythology, a **daemon** was a supernatural being of a nature between gods and humans whereas in the computer science realm a daemon is a computer program that runs as a background process rather than being under the direct control of an interactive user. A daemon thread in Python runs in the background. The difference between a regular thread and a daemon thread is that **a Python program will not exit until all regular/user threads terminate.** However, a program may exit if the daemon thread is still not finished.

For folks coming from Java background, this is similar to how JVM treats daemon threads. Daemon threads don't hold up a Java program from terminating.

**Marking a thread Daemon**

We can mark a thread daemon by passing in true for the `daemon` field in the `Thread` class's constructor.

**Creating a daemon thread**

```text
daemonThread = Thread(target=daemon_thread_task, daemon=True)
```

In the below snippet the main thread creates a **non-daemonic** thread and exits. However, the program continues to run. The code widget throws an error because the execution times out.

```python
from threading import Thread
from threading import current_thread
import time


def regular_thread_task():
    while True:
        print("{0} executing".format(current_thread().getName()))
        time.sleep(1)

regularThread = Thread(target=regular_thread_task, name="regularThread", daemon=False)
regularThread.start()  # start the regular thread

time.sleep(3)

print("Main thread exiting but Python program doesn't")
```

In contrast, the runnable snippet below will exit even though the **daemon** thread is still executing.

```python
from threading import Thread
from threading import current_thread
import time

def daemon_thread_task():
    while True:
        print("{0} executing".format(current_thread().getName()))
        time.sleep(1)

regularThread = Thread(target=daemon_thread_task, name="daemonThread", daemon=True)
regularThread.start()  # start the daemon thread

time.sleep(3)

print("Main thread exiting and Python program too")
```

Daemon threads are shut-down abruptly. Resources such as open files and database connections may not shut-down properly and daemon threads are not a good choice for such tasks. One final caveat to remember is that if you don't specify the `daemon` parameter in the constructor then the daemonic property is inherited from the current thread.

## Lock

Python's Lock is the equivalent of a Mutex and this lesson looks at its various uses.

#### Lock

Lock is the most basic or primitive synchronization construct available in Python. It offers two methods: `acquire()` and `release()`. A Lock object can only be in two states: **locked** or **unlocked**. A Lock object can only be unlocked by a thread that locked it in the first place.

A `Lock` object is equivalent of a mutex that you read about in operating systems theory.

**Acquire**

Whenever a Lock object is created it is initialized with the unlocked state. Any thread can invoke `acquire()` on the lock object to lock it. Advanced readers should note that `acquire()` can only be invoked by a single thread at any point because the GIL ensures that only one thread is being executed by the interpreter. This is in contrast to other programming languages with more robust threading models where multiple threads could be executing on different cores and theoretically attempt to acquire a lock at exactly the same time.

If a `Lock` object is already acquired/locked and a thread attempts to `acquire()` it, the thread will be blocked till the `Lock` object is released. If the caller doesn't want to be blocked indefinitely, a floating point timeout value can be passed in to the `acquire()` method. The method returns true if the lock is successfully acquired and false if not.

**Release**

The `release()` method will change the state of the Lock object to unlocked and give a chance to other waiting threads to acquire the lock. If multiple threads are already blocked on the acquire call then only one arbitrarily chosen \(varies across implementations\) thread is allowed to acquire the Lock object and proceed.

**Example**

An example is presented below where two threads share a list and one of the threads tries to modify the list while the other attempts to read it. Using a lock object we make sure that the two threads share the list in a thread-safe manner.

```python
sharedState = [1, 2, 3]
my_lock = Lock()
 
 
def thread1_operations():
    my_lock.acquire()
    print("{0} has acquired the lock".format(current_thread().getName()))
 
    time.sleep(3)  
    sharedState[0] = 777
 
    print("{0} about to release the lock".format(current_thread().getName()))
    my_lock.release()
    print("{0} released the lock".format(current_thread().getName()))
 
 
def thread2_operations():
    print("{0} is attempting to acquire the lock".format(current_thread().getName()))
    my_lock.acquire()
    print("{0} has acquired the lock".format(current_thread().getName()))
 
    print(sharedState[0])
    print("{0} about to release the lock".format(current_thread().getName()))
    my_lock.release()
    print("{0} released the lock".format(current_thread().getName()))
 
if __name__ == "__main__":
    # create and run the two threads
    thread1 = Thread(target=thread1_operations, name="thread1")
    thread1.start()
 
    thread2 = Thread(target=thread2_operations, name="thread2")
    thread2.start()
 
    # wait for the two threads to complete
    thread1.join()
    thread2.join()
```

If you examine the output from the above snippet, you'll notice that thread2 blocks on acquire method and waits for thread1 to release it before it can successfully acquire it.

**Deadlock Example**

Consider the example below, where two threads are instantiated and each tries to invoke `release()` on the lock acquired by the other thread, resulting in a deadlock.

```python
from threading import *
import time


def thread_one(lock1, lock2):
    lock1.acquire()
    time.sleep(1)
    lock2.release()


def thread_two(lock1, lock2):
    lock2.acquire()
    time.sleep(1)
    lock1.release()


if __name__ == "__main__":
    lock1 = Lock()
    lock2 = Lock()

    t1 = Thread(target=thread_one, args=(lock1, lock2))
    t2 = Thread(target=thread_one, args=(lock1, lock2))

    t1.start()
    t2.start()

    t1.join()
    t2.join()
```

#### Non-blocking lock

You might want to do something with a resource if it is not currently locked, but if it is locked you don't bother and do something else.

```python
import threading
import time

def worker(tasks):
  for task in tasks:
    time.sleep(0.001)
    if task.lock.acquire(blocking=True):
      print(f'{threading.current_thread}, {task.name} begins to start')
    else:
      print(f'{threading.current_thread}, {task.name} is working')

class Task:
  def __init__(self, name):
    self.name = name
    self.lock = threading.Lock()

tasks = [Task(f'task-{i}') for i in range(10)]
print(tasks)
for i in range(5):
  threading.Thread(target=worker, name=f'worker-{i}', args=(tasks,)).start()
```

Notice that if blocking set to true, '... is working' will not be printed out meaning everyone blocks there wait for the release of the lock. If it is set to False, then if it's failed to acquire a lock, it can continue execute code after the critical section.

## RLock

This lesson explains the reentrant lock in python's threading module.

#### Introduction

A reentrant lock is defined as a lock which can be reacquired by the same thread. A `RLock` object carries the notion of ownership. If a thread acquires a `RLock` object, it can chose to reacquire it as many times as possible. Consider the following snippet:

**Reentrant lock**

```python
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

```python
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

## Condition Variables

This lesson explains the concept of condition variables in Python.

#### Condition Variables

Synchronization mechanisms need more than just mutual exclusion; a general need is to be able to wait for another thread to do something. Condition variables provide mutual exclusion and the ability for threads to wait for a predicate to become true.

For Java programmers, the condition variable and its working may seem eerily familiar and rightly so because the threading module borrows a lot from Java's concurrency architecture. We'll examine the similarities towards the end of the lesson. First, let's try to understand why we need condition variables in the first place.

**Why**

In the previous sections we worked with locks which are used to enforce serial access to shared state. However, locks aren't enough when threads want to coordinate among themselves. Imagine a scenario where we have two threads working together to find prime numbers and print them. Say the first thread finds the prime number and the second thread is responsible for printing the found prime. The first thread \(finder\) sets a boolean flag whenever it determines an integer is a prime number. The second \(printer\) thread needs to know when the finder thread has hit upon a prime number. The naive approach is to have the printer thread do a busy wait and keep polling for the boolean value. Let's see what this approach looks like:

**Approach 1**

```python
def printer_thread_func():
    global prime_holder
    global found_prime
 
    while not exit_prog:
        while not found_prime and not exit_prog:
            time.sleep(0.1)
 
        if not exit_prog:
            print(prime_holder)
 
            prime_holder = None
            found_prime = False
 
 
def is_prime(num):
    if num == 2 or num == 3:
        return True
 
    div = 2
 
    while div <= num / 2:
        if num % div == 0:
            return False
        div += 1
 
    return True
 
 
def finder_thread_func():
    global prime_holder
    global found_prime
 
    i = 1
 
    while not exit_prog:
 
        while not is_prime(i):
            i += 1
 
        prime_holder = i
        found_prime = True
 
        while found_prime and not exit_prog:
            time.sleep(0.1)
 
        i += 1
 
 
found_prime = False
prime_holder = None
exit_prog = False
 
printer_thread = Thread(target=printer_thread_func)
printer_thread.start()
 
finder_thread = Thread(target=finder_thread_func)
finder_thread.start()
 
# Let the threads run for 5 seconds
time.sleep(3)
 
# Let the threads exit
exit_prog = True
 
printer_thread.join()
finder_thread.join()
```

Notice we aren't using any synchronization primitives in the program above. Not even locks to guard writes to shared variables. The program goes through two states, finding primes and then printing them, and repeats the sequence. The above program would work because there are only two threads involved but would fail with greater than two threads. Folks with Java background would realize that a similar program in Java wouldn't work given the memory model of Java, and would likely require the shared variables to be marked volatile.

The above program is essentially a producer-consumer problem. The printer thread is a consumer and the finder thread is a producer. The printer thread needs to be signaled somehow that a prime number has been discovered for it to print. Do you see a condition here? The condition in our program is the discovery of the prime number represented by the boolean variable `found_prime`. Realize that locks don't help us signal other threads when a condition becomes true.

One shortcoming of the above code is we have the printer thread constantly polling in a while loop for the `found_prime` variable to become true. This is called busy waiting and is highly discouraged as it unnecessarily wastes CPU cycles. Ideally, the printer thread should go to sleep so that it doesn't consume any system resources and be woken up when the condition it needs to act upon becomes true. This can be achieved through condition variables.

We create a condition variable as follows:

**Creating a condition variable**

```text
cond_var = Condition()
```

The two important methods of a condition variable are:

* `wait()` - invoked to make a thread sleep and give up resources
* `notify()` - invoked by a thread when a condition becomes true and the invoking threads want to inform the waiting thread or threads to proceed

A condition variable is always associated with a lock. The lock can be either reentrant or a plain vanilla lock. The associated lock must be acquired before a thread can invoke `wait()`or `notify()` on the condition variable.

**Incorrect way of using condition variables**

```text
cond_var = Condition()
cond_var.wait()  # throws an error
```

We can create a lock ourselves and pass it to the condition variable's constructor. If no lock object is passed then a lock is created underneath the hood by the condition variable. In the examples below we demonstrate all the different ways of invoking condition variables.

We can create the condition variable as follows:

**Creating a condition variable by passing a custom lock**

```text
lock = Lock()
cond_var = Condition(lock) # pass custom lock to condition variable
cond_var.acquire()
cond_var.wait()
```

We can also create a condition variable without passing in a custom lock.

**Creating a condition variable without passing a lock**

```text
cond_var = Condition()
cond_var.acquire()
cond_var.wait()
```

Given what we know so far about condition variables, let's try to rewrite the printer thread code from the previous section. Note, that we are checking for the boolean variables in an `if` statement.

**Printer thread refactored code**

```text
cond_var = Condition()
found_prime = False
prime_holder = None
exit_prog = False
 
def printer_thread_func():
    global prime_holder
    global found_prime
 
    while not exit_prog:
 
        # check for predicate
        cond_var.acquire()
        if not found_prime and not exit_prog:
            cond_var.wait()
        cond_var.release()
         
 
        if not exit_prog:
            print(prime_holder)
            prime_holder = None
            
            # acquire lock before modifying shared variable
            cond_var.acquire()
            found_prime = False
            # remember to wake up the other thread
            cond_var.notify()
            cond_var.release()
 
```

Note that we have rid ourselves of the busy wait loop. Now we check if the `found_prime` is false and if so we first acquire the lock on the condition variable and then invoke the `wait()` method on it. When the finder thread invokes `notify()` on the variable `cond_var`, that is the moment when the printer thread will wake up from its slumber and continue.

There are however two questions we need to answer:

* If the printer thread acquires the lock on the condition variable `cond_var` then how can the finder thread `acquire()` the lock when it needs to invoke the `notify()` method?
* Can the condition, which is the variable `found_prime`, change once the printer thread is woken up?

The answer to the first question is that when a thread invokes `wait()` it simultaneously gives up the lock associated with the condition variable. Only when the sleeping thread wakes up again on a `nofity()`, will it reacquire the lock.

The second question is very important and leads us to the correct idiomatic usage of the condition variable. The way we have nested the acquire and wait call under an if statement is incorrect. The reason is that if a thread invokes `notifyAll()` on a condition variable, then all the threads waiting on the condition variable will be woken up but only one thread will be allowed to make progress. Once the first thread exits the critical section and releases the lock associated with the condition variable, another thread, from the set of threads that were waiting when the original `notifyAll()` call was made, is allowed to make progress. This may not be appropriate for every use case and certainly not for ours if we had multiple printer threads. We would want a printer thread to make progress only when the condition `found_prime` is set to true. This can only be possible with a while loop where we check if the condition `found_prime` is true before allowing a printer thread to move ahead. The change will look as follows:

```text
    cond_var.acquire()
    while not found_prime and not exit_prog:
        cond_var.wait()
    cond_var.release()
```

On a side note, remember that a woken up thread reacquires the lock associated with the condition variable the thread was waiting on and doesn't give up the lock until it releases the condition variable. Hence, any statements executed between the `wait()` and the `release()` call would happen while the thread holds the lock assuring mutual exclusion within that block of code. Consider the below pseudocode:

```text
0.     cond_var.acquire()
1.     while predicate is not True:
2.         cond_var.wait()
3.    
4.     # code statement 1
5.     # code statement 2
6.     # code statement 3
7.        
8.     cond_var.release()
```

In the hypothetical snippet above, the **lines 2 to 8** would be executed by a woken up thread while holding the lock associated with the condition variable.

**Spurious Wakeups**

We may convince ourselves to not use a while loop if we only have a single printer thread in our program. However, a thread can be woken up even if there has been no notification!

A peculiarity of condition variables is the possibility of spurious wakeups. It means that a thread might wakeup as if it has been signaled even though nobody called `notify()` on the condition variable in question. This is specifically allowed by the POSIX standard because it allows more efficient implementations of condition variables under some circumstances. Such wakeups are called **spurious wakeups**.

A thread that has been woken up does not imply that the conditions for it to move forward hold. The thread must test the conditions again for validity before moving forward. In conclusion, we must always check for conditions in a loop and `wait()` inside it. The correct idiomatic usage of a condition variable appears below:

**Idiomatic use of wait\(\)**

```text
acquire lock
while(condition_to_test is not satisfied):
    wait
 
# condition is now true, perform necessary tasks
 
release lock
```

The idiomatic usage to notify\(\) is as follows:

**Idiomatic use of notify**

```text
acquire lock
set condition_to_test to true/satisfied
notify
release lock
```

