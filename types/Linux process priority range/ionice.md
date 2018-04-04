#  ionice - set or get process I/O scheduling class and priority 

SYNOPSIS
ionice [-c class] [-n level] [-t] -p PID...
ionice [-c class] [-n level] [-t] command [argument...]  
DESCRIPTION
This program sets or gets the I/O scheduling class and priority for a program. If no arguments or just -p is given, ionice will query the current I/O scheduling class and priority for that process.

When command is given, ionice will run this command with the given arguments. If no class is specified, then command will be executed with the "best-effort" scheduling class. The default priority level is 4.

As of this writing, a process can be in one of three scheduling classes:

Idle
    A program running with idle I/O priority will only get disk time when no other program has asked for disk I/O for a defined grace period. The impact of an idle I/O process on normal system activity should be zero. This scheduling class does not take a priority argument. Presently, this scheduling class is permitted for an ordinary user (since kernel 2.6.25). 
Best-effort
    This is the effective scheduling class for any process that has not asked for a specific I/O priority. This class takes a priority argument from 0-7, with a lower number being higher priority. Programs running at the same best-effort priority are served in a round-robin fashion.

    Note that before kernel 2.6.26 a process that has not asked for an I/O priority formally uses "none" as scheduling class, but the I/O scheduler will treat such processes as if it were in the best-effort class. The priority within the best-effort class will be dynamically derived from the CPU nice level of the process: io_priority = (cpu_nice + 20) / 5.

    For kernels after 2.6.26 with the CFQ I/O scheduler, a process that has not asked for an I/O priority inherits its CPU scheduling class. The I/O priority is derived from the CPU nice level of the process (same as before kernel 2.6.26).

Realtime
    The RT scheduling class is given first access to the disk, regardless of what else is going on in the system. Thus the RT class needs to be used with some care, as it can starve other processes. As with the best-effort class, 8 priority levels are defined denoting how big a time slice a given process will receive on each scheduling window. This scheduling class is not permitted for an ordinary (i.e., non-root) user. 