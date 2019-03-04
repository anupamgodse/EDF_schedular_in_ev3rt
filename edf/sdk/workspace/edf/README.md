#Modified Toppers Kernel with Preemptive EDF Scheduling
EV3 Mindstorm robot programmed with EV3RT

As per the assignment description, we tested the new kernel scheduler with the following task set: TA=(300, 100), TB=(500, 300), TC=(1500, 100) all with the same static priority.

The utilization of this system is exactly equal to one, so no new tasks can be added to the system. 100/300 + 300/500 + 100/1500 = 1

The application's 3 tasks handle the recording of eachother's behavior by communicating via global variables. While running, a task constantly sets the variable currently_running to its task ID. When it completes, a task will set that variable to IDLE. When a task starts running, it will check the value of currently_running, and if that vaiable is not IDLE, then the task knows that it has preempted another task, and will increment the appropriate counter and record the timestamp. All tasks are started simultaneously. 



The output shown below is an example of an EDF execution history for the given task set; the letters in the top row indicate that task was running in each 100 milisecond output. The row underneath indicates how many times the currently-running task has been invoked. Note that task 2 is preempted twice, with a separation of 1500 miliseconds. Because we are executing for 2 hyperperiods, all events should come in ultiples of 2, since the history repeats after 1 hyperperiod. In this case, our hyperperiod is 1500ms, and T2 gets preempted every 1500 miliseconds. 

EDF run for given task set:
	ABBBABABBACBBBAABBBABABBACBBB A
	11112232241333564447585592666(10)
	Task 1 never preempted
	Task 2 preempted 2 times
		@ 3300 ms
		@ 4800 ms
	Task 3 never preempted
	
	

The output shown below is an example of a RM execution history for the given task set; it is included because the assignment wants us to contrast it with the EDF history. Note the massively increased number of interrupts. This increase in context switching could have a negative effect on the system's performance in tighter time bands. 
	
RM run for the given task set:
	ABBABBABBABBABCABBABBABBABB  A BC
	111212322433531644745855966(10)62
	Task 1 never preempted
	Task 2 preempted 8 times
		@ 2000 ms
		@ 2300 ms
		@ 2600 ms
		@ 2900 ms
		@ 3500 ms
		@ 3800 ms
		@ 4100 ms
		@ 4400 ms
	Task 3 preempted 1 time
		@ 3200 ms
