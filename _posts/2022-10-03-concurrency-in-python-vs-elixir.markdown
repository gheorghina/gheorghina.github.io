
---
layout: post
title:  "Concurrency Models in Python CheatSheet"
date:   2021-02-01 00:18:23 +0700
categories: [concurrency, python]
---


## Parallelism

When two or more processes can run at the exact same time. This can happen, for instance, in multicore systems, where each process runs on a different processor. 


## Concurrency

When two or more processes try to run at the same time on top of the same processor. This is usually solved by techniques such as time slicing. 

However, these techniques do not execute in a truly parallel fashion. It just looks parallel to observers because of the speed at which the processor switches between tasks. 

The operative system will take care of scheduling time with the processor for each process that requires it. Then, it'll switch context between them, giving each one a slice of time.

No matter of the order of execution, the outcome shall be the same

|  | |
|---:|---|
| [CPU Scheduling](https://www.geeksforgeeks.org/cpu-scheduling-in-operating-systems/)  | an operating system module selecting next jobs and processes to run(uses context switch to stop and resume the context of a running thread or process) |
| [GIL - Python Global Interpreter Lock](https://realpython.com/python-gil/)  | a mutex (or a lock) that allows only one thread to hold the control of the Python interpreter at a time.  | 


## Use Cases

Adding multiprocessing support in python comes with an overhead. Therefore the use cases have to be justified for the introduced complexity.

|   | |
|---:|---|
| Multiprocessing - CPU Intensive Tasks  | string operations, search algo, graphics processing, number crunching, algo... |
| Async Programming - IO Intensive tasks  | pass the task to another Actor, get a callback once it is done; Ex: database read/writes, web service calls, copy, download, upload data to disk or a network, .. | 


## Thread vs Process

|   | |
|---:|---|
| Process  | is an instance of a computer program that is being executed. Any process has 3 basic components: an executable program, the associated data needed by the program (variables, work space, buffers, etc.) and the execution context of the program (State of process) |
| Thread | is an entity within a process that can be scheduled for execution; it is the smallest unit of processing that can be performed in an OS (Operating System) | 


## Threading

threading module( 1.5+) - threads are executed synchronously; managed by a mechanism called global interpreter lock 
Thread lifecycle: NEW - READY - BLOCKED - RUNNING - TERMINATED
Shared Memory and OS Resources( open files, network.) 

Cons: thread interference / race condition ( ex: bank account) - solved by threads synchronizations
Threads Synchronization
locking( lock - unlock -- by one single thread) --others threads get blocked until the lock is being released by the locking thread(python: thread.acquire / release) (con: deadlock -- release on finally of a try block / other option: with lock: statement )

## Multiprocessing

multiprocessing package (2.6+) - uses sub processes instead of threads ; 

## Abstract Concurrency

concurrent.futures(3.2+) - uses either thread pools OR sub process pools

## Asynchronous Programming 

asyncio module (3.4+)

## [asyncio](https://docs.python.org/3/library/asyncio.html)




## Resources

- [Wiki: Concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science))

- [Mastering Python High Performance by Fernando Doglio](https://www.goodreads.com/book/show/26781635-mastering-python-high-performance?ac=1&from_search=true&qid=tOP7V0juX8&rank=1)

- A good comprehensive list of concurrency models can be found in the book [Seven Concurrency Models in Seven Weeks: When Threads Unravel by Paul Butcher](https://www.goodreads.com/book/show/18467564-seven-concurrency-models-in-seven-weeks?ac=1&from_search=true&qid=zJFlYF2961&rank=1) 

- [Vectorization Alternatives in Python](https://pythonspeed.com/articles/vectorization-python-alternatives/)

- [Vectorization in Python](https://pythonspeed.com/articles/vectorization-python/)

- [Multithreading](https://realpython.com/python-pyqt-qthread/)

- [asyncio](https://docs.python.org/3/library/asyncio.html)

- [Pluralsight Python Concurrency](https://app.pluralsight.com/course-player?clipId=7c12a5e8-04b1-4243-943d-6d2d102e1b0d)

- [asyncio we did it wrong](https://www.roguelynn.com/words/asyncio-we-did-it-wrong/)

- [Waiting in asyncio](https://hynek.me/articles/waiting-in-asyncio/)