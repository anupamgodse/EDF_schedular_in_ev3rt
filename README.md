#Modified Toppers Kernel with Preemptive EDF Scheduling
EV3 Mindstorm robot programmed with EV3RT

Description: We modified the toppers kernel to implement a preemptive Earliest Deadline First scheduler for tasks of the same static priority. To do this, we had to modify the Task Control Block structure used by the kernel to keep track of a task's information to accomodate 2 additional variables; the current absolute deadline, and the number of times the task has been invoked. The period of the task is also no possible to access from within the act_tsk() function. The period and number of invocations are used by the kernel to compute the current absolute deadline. In theory, we would not need an actual deadline field, but it made inserting tasks into the queue significantly more convenient, as we did not have to recompute the deadline of each task we were comparing the new task's deadline to. 

The new EDF scheduler works by inserting itself into the runnable queue ahead of the first task it finds that has an absolute deadline greater than the new task. If no such task is found, and we reach the end of the queue, then the new task will be inserted at the end of the queue. After inserting the task into the queue, 


In all, 3 of the kernel files were changed in this assignment: task.c, task.h, task_manage.c

task.c was altered to have its make_runnable() function call the edf insert queue function instead of the basic one, and will replace the current about-to-be-executed task (p_schedtsk) if said task has a larger absolute deadline. The queue_insert_edf function was also added, and behaves as described above. 

task.h had the task_control_block modified to include the 3 additional fields described above.

task_manage.c has the act_tsk() function, which was altered to compute the absolute deadline of the task being activated, and store that value in the new field in the task's TCB. It must extract the period of the task from its corresponding Cyclical Control Block. 
