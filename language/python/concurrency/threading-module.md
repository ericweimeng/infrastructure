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

