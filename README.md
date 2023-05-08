Download Link: https://assignmentchef.com/product/solved-solvedprogramming-assignment-2-dns-name-resolution-engine
<br>
1 Introduction

In this assignment you will develop a multi-threaded application that resolves domain names to IP addresses, similar to the operation preformed each time you access a new website in your web browser. The application is composed of two sub-systems, each with one thread pool: requesters and resolvers. The sub-systems communicate with each other using a bounded queue. This type of system architecture is referred to as a Producer-Consumer architecture. It is also used in search engine systems, like Google. In these systems, a set of crawler threads place URLs onto a queue. This queue is then serviced by a set of indexer threads which connect to the websites, parse the content, and then add an entry to a search index. Refer to Figure 1 for a visual description.

2 Description

2.1 Name Files

Your application will take as input a set of name ﬁles. Names ﬁles contain one hostname per line. Each name ﬁle should be serviced by a single requester thread from the requester thread pool.

2.2 Requester Threads

The requester thread pool services a set of name ﬁles, each of which contains a list of domain names. Each name that is read from each of the ﬁles is placed on a FIFO queue. If a thread tries to write to the queue but ﬁnds that it is full, it should sleep for a random period of time between 0 and 100 microseconds.

2.3 Resolver Threads

The second thread pool is comprised of a set of THREAD MAX resolver threads. The resolver thread pool services the FIFO queue by taking a name oﬀ the queue and querying its IP address. After the name has been mapped to an IP address, the output is written to a line in the results.txt ﬁle in the following format: www.google.com,74.125.224.81

2.4 Synchronization and Deadlock

Your application should synchronize access to shared resources and avoid deadlock. You should use mutexes and/or semaphores to meet this requirement. There are at least two shared resources that must be protected: the queue and the output ﬁle. Neither of these resources is thread-safe by default.

2.5 Ending the Program

Your program must end after all the names in each ﬁle have been serviced by the application. This means that all the hostnames in all the input ﬁles have received a corresponding line in the output ﬁle.

3 What’s Included

Some ﬁles are included with this assignment for your beneﬁt. You are not required to use these ﬁles, but they may prove helpful.

1. queue.h and queue.c

These two ﬁles implement the FIFO queue data structure. The queue accepts pointers to arbitrary types. You should probably use the queue to store pointers to C-strings of hostnames. The requester threads should push these hostnames into the queue, and the resolver threads should obtain hostnames from the same queue. Please consult the queue.h header ﬁle for more detailed descriptions of each available function.

2. queueTest.c

This program runs a series of tests to conﬁrm that the queue is working correctly. You may use it as an example of how to use the queue, or to test the functionality of the queue code that you are provided.

2 3. util.c and util.h

These two ﬁles contain the DNS lookup utility function. This function abstracts away a lot of the complexity involved with performing a DNS lookup. The function accepts a hostname as input and generates a corresponding dot-formatted IPv4 IP address string as output. Please consult the util.h header ﬁle for more detailed descriptions of each available function.

4. input/names*.txt

This is a set of sample name ﬁles. They follow the same format as mentioned earlier. Use them to test your program.

5. results-ref.txt

This result ﬁle is a sample output of the IPs for the hostnames from the names*.txt ﬁles.

6. lookup.c

This program represents an un-threaded solution to this assignment. Feel free to use it as a starting point for your program, or as a reference for using the utility functions and performing ﬁle i/o in C.

7. pthread-hello.c

A simple threaded “Hello World” program to demonstrate basic use of the pthread library.

8. Makeﬁle A GNU Make makeﬁle to build all the code listed here.

4 Additional Speciﬁcations

Many of the speciﬁcations for your program are embedded in the descriptions above. This section details additional speciﬁcations to which you must adhere.

4.1 Program Arguments

Your executable program should be named “multi-lookup”. When called, it should interpret the last argument as the ﬁle path for the ﬁle to which results will be written. All proceeding arguments should be interpreted as input ﬁles containing hostnames in the aforementioned format. An example call involving three input ﬁles might look like: multi-lookup names1.txt names2.txt names3.txt result.txt

4.2 Limits

If necessary, you may impose the following limits on your program. If the user speciﬁes input that would require the violation of an imposed limit, your program should gracefully alert the user to the limit and exit with an error.

• MAX INPUT FILES: 10 Files (This is an optional upper-limit. Your program may also handle more ﬁles, or an unbounded number of ﬁles, but may not be limited to less than 10 input ﬁles.) 3

• MAX RESOLVER THREADS: 10 Threads (This is an optional upper-limit. Your program may also handle more threads, or match the number of threads to the number of processor cores.)

• MIN RESOLVER THREADS: 2 Threads (This is a mandatory lower-limit. Your program may handle more threads, or match the number of threads to the number of processor cores, but must always provide at least 2 resolver threads.) • MAX NAME LENGTH: 1025 Characters, including null terminator (This is an optional upper-limit. Your program may handle longer names, but you may not limit the name length to less than 1025 characters.)

• MAX IP LENGTH: INET6 ADDRSTRLEN (This is an optional upper-limit. Your program may handle longer IP address strings, but you may not limit the name length to less than INET6 ADDRSTRLEN characters including the null terminator.)

4.3 Error Handling

You must handle the following errors in the following manners:

• Bogus Hostname: Given a hostname that can not be resolved, your program should output a blank string for the IP address, such that the output ﬁle contines the hostname, followed by a comma, followed by a line return. You should also print a message to stderr alerting the user to the bogus hostname.

• Bogus Output File Path: Given a bad output ﬁle path, your program should exit and print an appropriate error to stderr.

• Bogus Input File Path: Given a bad input ﬁle path, your program should print an appropriate error to stderr and move on to the next ﬁle. All system and library calls should be checked for errors. If you encounter errors not listed above, you should print an appropriate message to stderr, and then either exit or continue, depending upon whether or not you can recover from the error gracefully.

5 External Resources

You may use the following libraries and code to complete this assignment, as well as anything you have written for this assignment:

• Any functions listed in util.h

• Any functions listed in queue.h

• Any functions in the C Standard Library

• Standard Linux pthread functions 4

• Standard Linux Random Number Generator functions

• Standard Linux ﬁle i/o functions If you would like to use additional external libraries, you must clear it with the TAs ﬁrst. You will not be allowed to use pre-existing thread-safe queue or ﬁle i/o libraries since the point of this assignment is to teach you how to make non-thread-safe resources thread-safe.


