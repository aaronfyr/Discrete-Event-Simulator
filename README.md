# Discrete-Event-Simulator
CS2030 Programming Methodology II AY 2021/22 Semester 2 Project\

## Narrative
A discrete event simulator is a software that simulates the changes in the state of a system across time, with each transition from one state of the system to another triggered via an event. Such a system can be used to study many complex real-world systems such as queuing to order food at a fast-food restaurant.

An event occurs at a particular time, and each event alters the state of the system and may generate more events. States remain unchanged between two events (hence the term discrete), and this allows the simulator to jump from the time of one event to another. Some simulation statistics are also typically tracked to measure the performance of the system.

The following illustrates a queuing system comprising a single service point S with one customer queue.

![Queuing System](https://i.imgur.com/xgh7ON0.png)

In this exercise, we consider a multi-server system comprising the following:

* There are a number of servers; each server can serve one customer at a time.
Each customer has a service time (time taken to serve the customer).
When a customer arrives (ARRIVE event):
if the server is idle (not serving any customer), then the server starts serving the customer immediately (SERVE event).
if the server is serving another customer, then the customer that just arrived waits in the queue (WAIT event).
if the server is serving one customer with a second customer waiting in the queue, and a third customer arrives, then this latter customer leaves (LEAVE event). In other words, there is at most one customer waiting in the queue.
When the server is done serving a customer (DONE event), the server can start serving the customer waiting at the front of the queue (if any).
If there is no waiting customer, then the server becomes idle again.
Notice from the above description that there are five events in the system, namely: ARRIVE, SERVE, WAIT, LEAVE and DONE. For each customer, these are the only possible event transitions:

ARRIVE → SERVE → DONE
ARRIVE → WAIT → SERVE → DONE
ARRIVE → LEAVE
In essence, an event is tied to one customer. Depending on the current state of the system, triggering an event will result in the next state of the system, and possibly the next event. Events are also processed via a queue. At the start of the simulation, the queue only contains the customer arrival events. With every simulation time step, an event is retrieved from the queue to be processed, and any resulting event added to the queue. This process is repeated until there are no events left in the queue.

As an example, given the arrival times (represented as a floating point value for simplicity) of three customers and one server, and assuming a constant service time of 1.0,

0.500
0.600
0.700
The simulation starts with three arrival events

<0.500 1 arrives>
<0.600 2 arrives>
<0.700 3 arrives>
The next event to pick is <0.500 1 arrives>. This schedules a SERVE event.

<0.500 1 serves by 1>
<0.600 2 arrives>
<0.700 3 arrives>
The next event to pick is <0.500 1 served>. This schedules a DONE event.
<0.600 2 arrives>
<0.700 3 arrives>
<1.500 1 done serving by 1>
The next event to pick is <0.600 2 arrives>...
This process is repeated until there are no more events.

Using our example, the entire simulation run results in the following output, each of the form <time_event_occurred, customer_id, event_details>
0.500 1 arrives
0.500 1 serves by 1
0.600 2 arrives
0.600 2 waits at 1
0.700 3 arrives
0.700 3 leaves
1.500 1 done serving by 1
1.500 2 serves by 1
2.500 2 done serving by 1
Finally, statistics of the system that we need to keep track of are:

the average waiting time for customers who have been served;
the number of customers served;
the number of customers who left without being served.
In our example, the end-of-simulation statistics are respectively, [0.450 2 1].

Priority Queuing
The main structure that is used to manage the events in any discrete event simulation is the Priority Queue. Java provides the PriorityQueue (a mutable class) that can be used to keep a collection of elements, where each element is given a certain priority.

Elements may be added to the queue with the add(E e) method. Note that the queue is modified;
The poll() method may be used to retrieve and remove the element with the highest priority from the queue. It returns an object of type E, or null if the queue is empty. Note that the queue is modified.
Take Note
The requirements of this project will be released in stages. For each level N, you will be given a driver class MainN in the file MainN.java, and you will be required to create the SimulateN class to go along with it.

All utility classes are to be packaged in cs2030.util, while the rest of the classes related to the simulation are to be packaged in cs2030.simulator, with SimulateN as the only public facing classes.

The Pair and ImList classes are provided.

Compile your code using

javac -d . *.java
and run your program to ensure that everything is still in good working order before proceeding to the next level.
