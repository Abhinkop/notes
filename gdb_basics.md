## Compilation for debugging
Add `-g` flag to add the debug symbols

## Start GDB
```bash
gdb ./myprog
```

## Attach to remote
### 1. Launch the gdb server 
for qemu add <br> 
`-s` : enables a GDB server on TCP port 1234 <br>
`-S` : freezes the guest at startup (does not start execution until continue).

### 2. Launch GDB
```bash
gdb-multiarch <optional symbol file name> # multiarch for cross debugging for ARM, if native use plain `gdb`
```
### 3. Connect to QEMU’s GDB server
```bash
(gdb) target remote :1234
```

## Basic commands
| Command                  | Description                                  |
| ------------------------ | -------------------------------------------- |
| `break main` or `b main` | Set a breakpoint at `main`                   |
| `break filename:line`    | Set breakpoint at specific file and line     |
| `run` or `r`             | Start the program                            |
| `next` or `n`            | Execute next line (skip into function calls) |
| `step` or `s`            | Step into function calls                     |
| `continue` or `c`        | Resume after breakpoint                      |
| `print x` or `p x`       | Print variable `x`                           |
| `backtrace` or `bt`      | Show call stack                              |
| `list` or `l`            | Show source code around current line         |
| `info locals`            | List local variables                         |
| `quit` or `q`            | Exit GDB                                     |
| `symbol-file symbol_file_name` | adding a symbol file                   |

## Inspecting Memory
Use the x (examine) command to view memory:
```bash
x/<count><format><size> <address>
```
`<count>`: Number of units to display

`<format>`: Output format:
`x` = hex
`d` = decimal
`u` = unsigned decimal
`t` = binary
`f` = float
`s` = string
`i` = instruction
`c` = char

`<size>`: Unit size:
`b` = byte
`h` = halfword (2 bytes)
`w` = word (4 bytes)
`g` = giant word (8 bytes)

`<address>`: Any valid expression (pointer, symbol, literal)

## Inspecting Registers
### View All Registers
```bash
info registers
```
### View Specific Register
```bash
print $reg # replace reg with acctual name
```

## Conditional Breakpoints in GDB
### Syntax
```bash
break LOCATION if CONDITION
```
Where:

`LOCATION` is a function name, file:line, or address.

`CONDITION` is any valid C-style expression using variables, registers, or memory.

### Eg:

#### Stop when a variable equals a value:
```bash
break my_function if count == 100
```
#### Stop at line 42 only when x > 10:
```bash
break main.c:42 if x > 10
```
#### Stop when the content at a memory address is specific:
```bash
break *0x80010000 if *(int*)0x80010000 == 0xDEADBEEF
```
#### Conditional on register value:
```bash
break *0x400800 if $x0 == 5
```

## TUI mode
### 1. Start GDB with TUI mode:
```bash
gdb -tui ./myprog
```

### 2. Or toggle TUI mode inside GDB:
Once inside GDB:

```bash
(gdb) layout src
```
Useful Layouts in TUI
> `layout src` — source code + command window <br>
> `layout asm` — assembly + command window <br>
> `layout regs` — registers + command window <br>
> `layout split` — source + assembly <br>

You can also switch between them at any time during your session.

Navigation Keys (TUI Mode)
|Key                |Action                     |
|-------------------|---------------------------|
|`Ctrl` + `L`	    |Refresh/redraw screen      |
|`Ctrl` + `X` + `A`	|Toggle TUI mode on/off     |
|`Page Up`/`Down`	|Scroll source code         |
|`Arrow Keys`	    |Move cursor in command line|