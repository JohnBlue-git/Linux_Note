## GDB command for debugging

### Before GDB:
create program with debug mode (via plain g++ command)
```console
g++ -g -o trigger_core_dump trigger_core_dump.cpp
```
create program with debug mode (via cmake)
```console
# set via cmake script
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
endif()

# set via cmake command
cmake -DCMAKE_BUILD_TYPE=Debug

# Debug or Release
```
create program with debug mode (via meson)
```console
# set via meson script
if get_option('buildtype') == 'debug'
  message('Building in debug mode')
elif get_option('buildtype') == 'release'
  message('Building in release mode')
endif

# set via meson command
meson setup builddir --buildtype debug
```

### Starting GDB:
```console
# normal
gdb <executable>

# attach pid
# 1:
gdb -p 12271
# 2:
gdb /path/to/exe 12271
# 3:
gdb /path/to/exe
(gdb) attach 12271
```
### Setting Breakpoints:
```console
break <line_number> or br <line_number>
break <function>
break <file|swap.c>:<line_number>
```
### Setting arguments:
```console
set variable x = 10
set args <arg1> <arg2>
set environment PATH = /usr/local/bin
```
### Running the Program (until breakpoint or finish):
```console
run or r

# with arguments
r <arg1> <arg2> ...

# stop at main()
start <arg1> <arg2> ...
```
### Stepping Through Code:
```console
next or n
nexti or ni

step or s
stepi or si

continue or c

# continue running until finish
finish
```
### Examining the Program State:
```console
# print
print <variable> or p <variable>

# Show the global and static variable names
info variables

# Show the values of local variables in the current scope
info locals

# List all breakpoints that have been set
info breakpoints
info b

# Print the current call stack of the program
backtrace or bt
```
### Managing Breakpoints:
```console
delete <id>
disable <id>
enable <id>
clear <line_num>
clear <file>:<line_num>
clear
```
### Others:
```console
quit or q
help or h

# List source code around the current line
list or l
```

### Work with core dump:
enable core dump
```console
# enable it
ulimit -c unlimited

# verify it is now set to "unlimited"
ulimit -c
```
create program
```console
g++ -g -o trigger_core_dump trigger_core_dump.cpp
```
run program
```console
./trigger_core_dump
```
the core dump file will typically be named core or core.<PID> \
, or another name defined in /proc/sys/kernel/core_pattern \
(But the location may vary ... or cannot found) \
(Ref: https://askubuntu.com/questions/1349047/where-do-i-find-core-dump-files-and-how-do-i-view-and-analyze-the-backtrace-st)
```console
cat /proc/sys/kernel/core_pattern
# |/usr/share/apport/apport -p%p -s%s -c%c -d%d -P%P -u%u -g%g -- %E
# Here's a breakdown of the placeholders:
# %p - Process ID
# %s - Signal number
# %c - Core file size (in bytes)
# %d - Date of the dump
# %P - Parent process ID
# %u - User ID of the dumped process
# %g - Group ID of the dumped process
# %E - Executable name
```
check core dump
```console
gdb ./trigger_core_dump <path/to/core dump file>
```
inspect the Core Dump (to display the backtrace)
```console
(gdb) bt
```

### Can gdb step into a detached or joined thread?
The answer is No !
- Detached Threads: \
  When you detach a thread, gdb will no longer be able to control it. You can't step into a detached thread because it’s been let go by the debugger. This is like saying goodbye to a friend who’s gone off-grid—no more tracking their every move.
- Joined Threads: \
  If a thread has already completed its task and you join it, that thread has exited and is no longer active. gdb cannot step into it because it doesn't exist in an active state. (Tough the core dump is still trackable)
- For example: \
  The following program and core dump is not trackable \
  g++ -g -o trigger_core_dump -pthread trigger_core_dump.cpp \
  ```c++ 
  #include <iostream>
  #include <pthread.h>
  
  void* proccessThd(void* nan) {
      int* p = nullptr; // Null pointer
      std::cout << *p;  // Dereferencing null pointer (will cause a segmentation fault)
      pthread_exit(nullptr);
  }
  
  int main() {
      pthread_t thd;
      pthread_create(&thd, nullptr, proccessThd, nullptr);
      pthread_detach(thd);
      return 0;
  }
  ```
  The code with joined thread is somewhat trackable
  ```c++ 
  #include <iostream>
  #include <pthread.h>
  
  void* proccessThd(void* nan) {
      int* p = nullptr; // Null pointer
      std::cout << *p;  // Dereferencing null pointer (will cause a segmentation fault)
      pthread_exit(nullptr);
  }
  
  int main() {
      pthread_t thd;
      pthread_create(&thd, nullptr, proccessThd, nullptr);
      pthread_join(thd, nullptr);
      pthread_exit(nullptr);
      return 0;
  }
  ```

  ### Useful references
  http://www.study-area.org/cyril/opentools/opentools/x1253.html

