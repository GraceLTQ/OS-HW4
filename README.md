# OS-HW4

# The Project
The lectures and the Harmony book describe a concurrent queue. You are to develop a similar abstraction called a “double-ended queue,” commonly known as a deque (pronounced “deck”). Like a queue or a stack, a deque is a dynamic list of elements. But unlike those abstractions, you can add elements to either side and remove elements from either side. A deque can double as a FIFO queue and as a stack.

Start with a specification, which must have the following methods: Deque(): returns the initial value of an empty deque.
- put left(d, v): d points to a deque and v is some value. Put v at the left-end of the deque. Does not return a value.
- put right(d, v): d points to a deque and v is some value. Put v at the right-end of the deque. Does not return a value.
- get left(d): d points to a deque. If the deque is empty, the operation should return None. Otherwise it should remove and return the value at the left-end of the deque.
- get right(d): d points to a deque. If the deque is empty, the operation should return None. Otherwise it should remove and return the value at the right-end of the deque.

Place the entire specification in file deque.hny. Note that all operations except for Deque() should be atomic. Please make sure the operations are named exactly as above, or the autograder will not be able to find them.

The deque implementation, which you should put in file deque impl.hny, should define the same methods but cannot use the atomically or sequential keywords and should instead rely on Lock, acquire, and release from the synch module (which the deque impl.hny file should import). As in the queue implementation, the contents of the deque should be implemented using a linked list, with nodes allocated using malloc and free from the alloc module. All operations should be O(1).

You also should create a test program, in file deque test.hny, for your deque. Note that the test should be a black box test—it cannot look inside the implementation of the deque. We should be able to run your test program with somebody else’s implementation of the deque. So, you shouldnot deference the deque pointer to see what’s in it—you can only use the deque interface in your test program.

Testing should be based on comparing behaviors between the specification and the implemen- tation (a form of differential testing). Model your test program after the queue btest1.hny test program from the Harmony book. It should import deque, not deque impl. It should allocate a single deque using Deque(). The program should define the following constants, all initialized to 1 in the program:
- N PUT LEFT: the number of threads that execute a put left operation.
- N PUT RIGHT: the number of threads that execute a put right operation.
- N GET LEFT: the number of threads that execute a get left operation.
- N GET RIGHT: the number of threads that execute a get right operation.

For example, the program should spawn N GET LEFT threads, uniquely identified through a param- eter self, each of which executes get left(d) on the deque d. Before the operation, the thread should print its intention to execute the operation. After the operation, the thread should print that the operation has finished and what the return value was (not needed for the put operations). We recommend that put left threads put the tuple (self, "left") on the queue, while put right threads put the tuple (self, "right") on the queue (instead of just self ).

Use the harmony flags -o deque.hfa and -B deque.hfa to compare the behaviors of the deque specification and the implementation. That is, you might run, for example:
- `harmony -c N_PUT_LEFT=2 -c N_PUT_RIGHT -o deque.hfa deque_test.hny`
- `harmony -c N_PUT_LEFT=2 -c N_PUT_RIGHT=2 -B deque.hfa -m deque=deque_impl deque_test.hny`
  
You can try different numbers of threads of each type, but beware that model checkers suffer from state explosion, and thus going much beyond 6 threads total may not be feasible. (You can also, if you like, pre-populate your test deque before starting the threads.)

# Submitting your work
You have to submit deque.hny, deque impl.hny, and deque test.hny to CMSX. If you work in a group, submit your work as a single group in CMSX.
