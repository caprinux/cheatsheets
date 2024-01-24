## THE GNU DEBUGGER

### Motivation

- GDB is a debugger primarily used in debugging ELF/Linux programs
- The end goal is to have full read/write control over the memory at any point of execution to aid in analysis
- GDB is powerful and even supports scripting. This list is in no way exhaustive of the capabilities of GDB.

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


## Breakpoints, Tracing, Watchpoints

### Breakpoints

> ELI5: Think of a breakpoint as a bookmark. When you place a bookmark in a book and flip through a book, it allows you to quickly and easily stop at that specific page in the book, to read an analyze the contents. 
>
> Similarly, we can set a breakpoint in a program such that the program will be paused when it tries to execute the instruction at the breakpoint. This allows us to inspect/modify the contents of the memory and the state of the program at that point of execution.

| command | description |
| --- | --- |
| (b)reak *<func_name/address> | set a breakpoint  |
| info break | view the list of breakpoints |
| disable <breakpoint number> | disable a specified breakpoint |
| enable <breakpoint number> | enable a specified breakpoint |
| delete <breakpoint number> | delete the breakpoint |

### Tracing

Sometimes, we might just want to just want to extract certain information from a breakpoint and we do not need to necessarily stop the program there.

An example is that we can use breakpoints as a hook to print arguments that are passed into a specific function.

```
break *malloc
commands
silent
printf "x is %d\n",x
cont
end
```

Alternatively, we can also go one step further and use the **dprintf** command.

```
dprintf *malloc, "%x\n", $rdi
```

which prints out the argument passed to **malloc**.

### Watchpoints

Another problem we might encounter is that we want to find how a certain part of the memory is being modified. We want to breakpoint on all instructions that modify the memory at address XXX.

| commands | description |
| --- | --- |
| awatch *<address> | break when specified memory is accessed |
| rwatch *<address> | break when value at specified memory is read/written to |

## Memory Manipulation

We can also view and interpret memory in the program in many ways. One main way is using the **examine** command which follows the following syntax

`x/<num><size><format>`

We can seek the documentation using the `help x` command in gdb.

> Examine memory: x/FMT ADDRESS.8RESS is an expression for the memory address to examine.
> FMT is a repeat count followed by a format letter and a size letter.
> Format letters are o(octal), x(hex), d(decimal), u(unsigned decimal),
>   t(binary), f(float), a(address), i(instruction), c(char), s(string)
>   and z(hex, zero padded on the left).
> Size letters are b(byte), h(halfword), w(word), g(giant, 8 bytes).
> The specified number of objects of the specified size are printed
> according to the format.  If a negative number is specified, memory is
> examined backward from the address.
> 
> Defaults for format and size letters are those previously used.
> Default count is 1.  Default address is following last thing printed
> with this command or "print".

Here are some examples:

| command | description |
| --- | --- |
| x/5gx <address> | view **5** giant-sized (aka QWORD) hex at specified address |
| x/20wd <address> | view **20** word-sized (aka DWORD) hex at specified address |
| x/s <address> | view string at specified address |

## Miscellaneous Information

Apart from providing specific addresses, we can also use other values such as register values by prepending it with a `$` sign _(i.e. x/gx $rsp allows us to inspect the first QWORD hex on the stack)_
