# lovepreet
Multiple-Reader-writer
The multiple user reader writer problem wherein multi- ple “writers” can write to a shared queue and multiple “readers” can read data off the shared queue with no race conditions. While a reader “writes” a certain element to the queue, no “reader” can read that element. The readers may read other elements but not the one current written to. Similarly, only one “writer” can update the queue at a time. Multiple “readers” may read elements off the queue but dequeuing from the queue by the “readers” must be atomic.

Description Of Code
Main function :
Creates a new Queue.
Initialises 2 semaphores, namely mutex and wrt. "mutex" is used for keeping track of the no, of readers, whereas "wrt" can be regarded as lock on the queue.
Pthreads are then created according to the user input.
Reader function : Serves the reading purpose of multiple readers via menu.
The reader first tries to get "mutex" via "sem_wait", in order to tell that first reader has arrived.
If it gets it, the readercount is incremented by that process only.
If the process is the first process to read from the shared queue, then "wrt" semaphore is also acquired so as to prevent the writers from updating the contents of the queue at that time.
Reader menu provides (based upon the choice of the user): i) Printing entire queue elements, except the current written one. (Call to the PrintPrevQ() function). ii) Printing an element based upon the index entered by the user, except the current written one. (1-based indexing is followed, Call to the PrintQElement() functiom). iii) Reader can quit anytime.
"sem_post" on "mutex" allows other reader process to read from the queue.
Again the "mutex" is acquired so as to now decrement the readercount.
If no readers are left, "wrt" lock on the shared queue is lifted to allow writers to write.
Lock on "mutex" is also lifted as no readers are left now.
Above procedure also maitains atomicity among the readers.
Writer function : Serves the writing purpose of multiple writers via menu.
First the lock on "wrt" is acquired, signalling that a writer process has arrived and only 1 writer at a time can do the updation.
Writer menu provides (based upon the choice of the user): i) Proper message is received from the user and enQueue() function is called. ii) Writer can quit anytime.
Lock on "wrt" is lifted, signalling that now the lock on shared queue can be acquired by other writer processes.
PrintPrevQ function : Prints the contents of the shared queue, except the last written one. (Performs reading)

enQueue function : Enqueues a new message by a writer into the shared queue.

PrintQ function : Prints all the contents of the shared queue.

PrintQElement function : Prints an element from the queue based upon its index.

Compiling And Testing
Ensure that the Makefile and the source code are in the same directory.
In the terminal, type: 1. make 2. ./rw
To terminate the program, quit from all the reader-writer threads. (Option is provided in the menu).
Inputs To Give
The code is user-friendly (No need to give arguments at command line).

Enter the no. of reader and writer threads (Must be between 1 and 100 [inclusive]).
Provide proper messages when asked.
Provide proper choice for menus as is asked.
Provide proper index while printing a specific element. 1-based indexing is followed. Since the reader cannot access the current written element, do not provide the index equal to queue size (will give Element Access Error).
Expected Output
Only 1 writer should write to the shared queue at a time.
Multiple readers should read from the shared queue.
Messages read by the readers should not be the current written message, irrespective of the writer.
Specific messages that readers want to read from the queue.
Menu-driven functionality is provided.
Pthreads may generate random outputs (Outputs need not be same after each run), but above rules should follow.
Error Value And Interpretation
Scanf Error:- If argument provided by the user is 1. Not a number 2. A number < 1 or > 100 (when providing No. of reader/writers)

Message Error:- If the message provided by the user is newline.

Index Error:- If the index entered while seeking a message at a specific index in the shared queue is < 1 or > the queue size.

Element Access Error:- If the index entered while seeking a message at a specific index in the shared queue is equal to the queue size since seeking current written elements is not allowed.

Input Error:- If the choice entered is not what is expected.
