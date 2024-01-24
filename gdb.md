## THE GNU DEBUGGER

### Motivation

- GDB is a debugger primarily used in debugging ELF/Linux programs
- The end goal is to have full read/write control over the memory at any point of execution to aid in analysis

### Extensions

I highly recommend using the following GDB extensions to enhance your debugging experience.

This tutorial asumes that you have one of the extensions installed.

- [PwnDbg](https://github.com/pwndbg/pwndbg)
- [GEF](https://github.com/bata24/gef)
- [Peda](https://github.com/longld/peda) (deprecated)

## Program Flow

The following commands are commands that work with vanilla GDB.

| command | description |
| --- | --- |
| (r)un <args> | begin execution of programs (with specified arguments) from the beginning |
| ni | execute the **n**ext **i**nstruction in the program |
| si | execute the next instruction in the program, **s**tep **i**nto function calls if any |
| (fin)ish | execute until it has returned from/finished the current function |
| bt / backtrace | view the call stack of the program |


The following commands are commands that only work with GDB extensions (tested on GEF).

| command | description |
| --- | --- |
| init/start/main <args> | begin execution of programs (with specified arguments) from the beginning, up till the main function or the entrypoint of the program |
| nextret | execute until the next return instruction |


## Breakpoints

## Memory Manipulation
