
## PDB command for python debugging

### Before PDB:
create .py with breakpoints
```python
import pdb

def add(a, b):
    pdb.set_trace()  # Set a breakpoint here
    return a + b

def main():
    x = 5
    y = 10
    pdb.set_trace()  # Set a breakpoint here
    result = add(x, y)
    print(result)
main()
```

### Starting PDB:
```console
# shalle run in debug mode
python3 -m pdb <script name>.py
```

### Running the Program:
Here's a quick overview of some useful pdb commands for stepping through code:
```console
# Execute the next line of code, but do not step into functions.
n (next)

# Step into the function call on the current line.
s (step)

# Continue execution until the next breakpoint.
c (continue)

# List the current location in the script.
l (list)

# Print the value of a variable.
p (print)

# Quit or Help
q (quit)
help
```

### Can PDB step into a detached or joined thread?
The Python Debugger (pdb) does not support stepping into threads that have been detached or joined.
When you use threads in Python, the pdb debugger can only follow the main thread of execution.
It does not have the capability to switch between threads or debug multiple threads concurrently.
\
However, we can still use the debugger to step through the thread, via placing the pdb breakpoint within the specific function related to the thread.
