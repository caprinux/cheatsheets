## PWNTOOLS -- The Pwn Scripting Library

### Motivation

- Pwntools is a python module that is used to automate the interaction with both local and remote programs.
- Built on top of the python sockets and subprocess library, it aims to provide an easy-to-use API to script program interaction, primarily to aid in binary exploitation.
- This cheatsheet takes a learn-by-example approach, hoping to bring across the idea of how to use the pwntools library efficiently through sample scripts.

## Basic Usage

```py
# for simplicity sake, we use a wildcard import when writing short scripts
from pwn import *

p = process("/bin/echo") # we can run local programs like this
p = remote("127.0.0.1", 4444) # we can connect to remote processes by providing the IP and PORT

# we can programtically interact with the process using the following APIs
p.sendline(b"Hello World!") # sends the provided string, followed by a newline ('\n') character
print(p.recvline()) # receives output from the program up till the newline ('\n') character

# apart from using sendline and recvline, we can also use send and recv to do I/O without the newline character
p.send(b"Hello World!\n") # this is equivalent to the sendline above
p.recv(10) # need to specify number of bytes to receive
p.recvuntil(b"\n") # same as p.recvline()

# even easier, we can do a p.sendlineafter if we want to reply to some sort of a prompt
p.sendlineafter(b"What is your name?", b"Ahmed")

# similar to how we would run the program in the command line,
# or how we would connect to remote processes using netcat,
# we can interact with the program directly using the interactive() function
p.interactive()
```

## Advanced Usage: ELF Module

```py
from pwn import *

print(elf.functions['main'].disasm()) # we can even get the disassembly of a function

# important stuff
print(elf.got) # entries in the global offset table and the addresses
print(elf.sym) # dictionary of all symbols and their addresses in the program (symbols include functions and global variables etc.)

# assuming no PIE, get address of main
print(elf.sym.main)

# if there is PIE, it will give us the offset of main from the random base address.
# however, if we know the base address i.e 0x400000, we can set it in our script
elf.address = 0x400000
print(elf.sym.main)
```

## Helper Modules

**todo**

- assembler
- xor
- rop module

