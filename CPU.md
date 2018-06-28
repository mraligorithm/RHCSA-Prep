# CPU
- - - -
#### Identify CPU/Memory Intensive Processes, Adjust Priority, Kill Processes
* top
»» k * Kill process
»» q * Quit
»» r * Renice
»» s * Change update rate
»» P * Sort by CPU usage
»» M * Sort by memory usage
»» l * Toggle load average
»» t * Toggle task display
»» m * Toggle memory display
»» B * Bold display
»» u * Filter by username
»»  -b * Start in batch mode
»»  -n * Number of updates before exiting
»» Columns:
— PID  * Process ID
— USER
— PR  * Priority
— RES  * Non-swap memory
— SHR  * Shared memory size
— %CPU  * Task’s share of elapsed CPU time
— %MEM  * Current amount of used memory
— TIME+  * CPU time minus the total CPU time the task has used since starting

* Nice priority:
»» -20  * Highest priority
»» 19  * Lowest priority
»» Any user can make a task lower priority

* `pgre`p * Search processes
»»  -u * Username
»»  -l * Display process name
»»  -t * Define tty ID
»»  -n * Sort by newest
* `pkill` * Kill process
»»  -u * Kill process for defined user
»»  -t * Kill process for defined terminal

* Kill signals:
»» 1  * SIGHUP  * Configure reload without termination; also used to report termination of
controlling process
»» 2  * SIGINT  * Cause program to terminate
»» 3  * SIGQUIT  * When user requests to quit a process
»» 9  * SIGKILL  * Immediately terminate process
»» 15  * SIGTERM  * Send request to terminate process; request can be interpreted or ignored
»» 18  * SIGCONT  * Restart previously stopped process
»» 19  * SIGSTOP  * Stop a process for later resumption
»» 20  * SIGTSTP  * Send by terminal to request a temporary stop
* ps * Process status
- - - -

#linux/commands