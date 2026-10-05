# Computing Roadmap

This roadmap is a guide for continuous, self-directed learning focused on understanding computing from the ground up. Inspired by a low-tech approach, the goal is to look beyond modern high-level frameworks and black-box abstractions to grasp how software interacts with memory and the operating system through classic books, UNIX manual pages, paper-based design, and building raw C utilities from scratch.

## Module 01: Terminal & Local Tools

### Navigation & File Manipulation via CLI

#### Why Study This
GUI file managers hide how the operating system structures, addresses, and controls access to data. Mastering the command line teaches you absolute vs. relative paths, standard streams (`stdin`, `stdout`, `stderr`), file permissions, and how the OS passes parameters to programs via environment variables and arguments.

#### Media
- The Linux Command Line (William Shotts)
- Manual pages (man)

#### Method
Mouse-free daily operation. Perform all file, directory, and text manipulations strictly through terminal commands.

#### Practical Project
**Workspace Scaffolder (`mkproject`)**
Write a POSIX Shell script (`mkproject`) that automates the setup of a standardized C project workspace. The script must accept a project name as an argument, check if the directory exists, set up a canonical folder hierarchy (`src/`, `include/`, `build/`, `docs/`), generate a boilerplate `README.md` with metadata, and apply appropriate UNIX permissions (`chmod 755` for directories, `644` for files).

## Module 02: C Programming, Tooling & Memory

### 1. Fundamental Logic & Standard I/O

#### Why Study This
Modern interpreted languages hide memory bounds, variable sizing, and CPU execution costs. C forces you to understand data types at the byte level, how data is read from standard input, and how memory is laid out sequentially in stack frames.

#### Media
- C Primer Plus (Stephen Prata)
- The C Programming Language (Kernighan & Ritchie)

#### Method
Design program logic and flowcharts on paper before typing code. Compile using strict flags (`gcc -Wall -Wextra -std=c99`) without autocompletion plugins.

#### Practical Project
**Custom Text Inspector (`textstat`)**
Build a command-line tool that processes plain text fed via standard input (`stdin`) or file paths. It must parse the stream character-by-character using `getchar()` or `fgetc()`, count total lines, words, bytes, and printable characters, and display a formatted frequency summary of alphanumeric characters directly in the terminal.

### 2. Terminal-Based Version Control (Git)

#### Why Study This
Git is a local content-addressable filesystem and DAG (Directed Acyclic Graph) engine, not just a cloud upload tool. Understanding Git via the CLI teaches you how software revisions are stored as snapshots (blobs, trees, commits) without relying on visual IDE abstractions.

#### Media
- Pro Git (Scott Chacon & Ben Straub)

#### Method
Manage all repository operations exclusively from the terminal. Focus on atomic commits and descriptive commit messages.

#### Practical Project
**Local Revision Control Integration**
Initialize a local Git repository within your workspace. Track and version your `textstat` project entirely from the command line. Create feature branches to refactor parsing logic, inspect local diffs using `git diff`, view history using `git log --oneline --graph`, and practice resolving merge conflicts manually in a terminal text editor (e.g., `vim` or `nano`).

### 3. Pointers & Manual Memory Management

#### Why Study This
Automatic garbage collection conceals how computers store and access memory. Mastering pointers, address arithmetic, and dynamic allocation (`malloc`/`free`) is essential for understanding how the CPU references locations in RAM, how the Stack differs from the Heap, and how to prevent fatal runtime memory leaks or segmentation faults.

#### Media
- Modern C (Jens Gustedt)
- Paper-based memory diagrams

#### Method
Draw explicit memory maps (stack frames, heap addresses, pointer references) on paper before writing code.

#### Practical Project
**Dynamic Hex Viewer & Memory Dump (`hexview`)**
Build a command-line utility that opens any binary or text file, dynamically allocates a heap buffer to load chunks of bytes, and prints a formatted hexadecimal and ASCII inspection grid (similar to `hexdump -C`). The program must accept custom buffer sizes via CLI arguments, reallocate memory as needed using `realloc()`, and explicitly free all heap memory before exiting.

### 4. Build Automation with Makefiles

#### Why Study This
Manually invoking `gcc` becomes error-prone as projects grow to multiple files. `make` teaches you how build systems evaluate file modification timestamps to construct a Directed Acyclic Graph (DAG) of dependencies, recompiling only what has changed.

#### Media
- GNU Make Manual
- Manual pages (man make)

#### Method
Deconstruct projects into clean headers (`.h`) and implementation files (`.c`). Write `Makefiles` from scratch without external generators.

#### Practical Project
**Modular Utility Build Pipeline**
Refactor `hexview` into a multi-file architecture (`main.c` for CLI flag parsing, `display.c` for hex formatting, and `display.h` for function prototypes). Write a clean `Makefile` defining targets (`all`, `clean`), variables (`CC`, `CFLAGS`), and explicit object file compilation rules (`.o`). Ensure that modifying a header file triggers re-compilation of dependent modules only.

## Module 03: Data Structures & Algorithms

### 1. Dynamic Data Structures (Lists, Stacks, Queues)

#### Why Study This
Built-in language arrays have fixed limits or hidden resizing overheads. Implementing dynamic data structures from scratch using pointers teaches you how memory nodes are linked together in the heap, how to manage pointers safely during insertions and deletions, and how to prevent memory corruption.

#### Media
- Grokking Algorithms (Aditya Bhargava)
- Mastering Algorithms with C (Kyle Loudon)

#### Method
Draw node links and pointer shifts on paper before writing code. Trace null pointers using `gdb` or structured debugging prints.

#### Practical Project
**CLI In-Memory Task Ledger (`todo`)**
Build a lightweight task manager that stores entries in a dynamically allocated Singly Linked List in memory. Users can append tasks, mark items as completed (which deletes nodes and updates adjacent pointers), and list active tasks. Include a persistence feature that parses and writes the list state to a local plain-text configuration file (`.tasks.txt`) upon startup and exit.

### 2. Algorithmic Efficiency & Sorting

#### Why Study This
Code that works fine on 10 items can freeze a system when processing 1,000,000 items. Studying time complexity ($O(n)$ vs $O(n \log n)$) gives you a mathematical framework to predict performance bottlenecks and choose optimal algorithms based on data volume.

#### Media
- Grokking Algorithms (Aditya Bhargava)

#### Method
Measure execution time across various input sizes using hardware clock counters (`<time.h>`).

#### Practical Project
**Benchmarking Sorting Engine (`sortbench`)**
Implement both Bubble Sort $O(n^2)$ and Quick Sort $O(n \log n)$ from scratch in C. Build a benchmark CLI tool that generates synthetic datasets of random integers (ranging from 1,000 to 100,000 elements), runs both algorithms on identical arrays, and prints a side-by-side performance table showing execution times in milliseconds.

## Module 04: Operating Systems & Networking

### 1. System Calls & POSIX File I/O

#### Why Study This
Standard library functions like `fopen()` and `printf()` are buffered wrappers provided for convenience. Using low-level system calls (`open`, `read`, `write`, `close`) exposes the raw boundary between user-space programs and the operating system kernel, teaching you how file descriptors operate.

#### Media
- Modern Operating Systems (Andrew Tanenbaum)
- Manual pages (man 2 system calls)

#### Method
Interact directly with kernel system call wrappers. Monitor system call invocations using `strace`.

#### Practical Project
**Rebuilding the `cat` Utility (`mycat`)**
Write a low-level file printing tool using POSIX system calls (`open`, `read`, `write`, `close`). The program must read input from file paths passed as arguments or directly from `stdin` (file descriptor `0`), process data through a raw fixed-size byte buffer, and write directly to `stdout` (file descriptor `1`).

### 2. Process Lifecycle & Execution Control

#### Why Study This
Operating systems run multiple programs concurrently by isolating memory spaces into processes. Learning `fork()`, `exec()`, and `wait()` unveils how the kernel duplicates process state, loads executable binaries into memory, and manages parent-child process execution flows.

#### Media
- Modern Operating Systems (Andrew Tanenbaum)

#### Method
Diagram process trees (PIDs and Parent PIDs) on paper to visualize parallel execution paths.

#### Practical Project
**Minimalist UNIX Command Shell (`microsh`)**
Build a custom terminal shell loop that displays a prompt, reads a user command string, parses token arguments, and invokes `fork()` to create a child process. The child uses `execvp()` to execute standard system binaries (like `ls` or `mycat`), while the parent process uses `waitpid()` to suspend execution until the child process completes and returns its exit code.

### 3. Network Sockets & Stream Communication

#### Why Study This
The internet relies on network sockets, which abstract network communication as streaming file descriptors. Building a socket application from scratch teaches you the TCP/IP lifecycle (`socket`, `bind`, `listen`, `accept`), how ports are opened on the host, and how protocol text frames (like HTTP) are parsed byte-by-byte.

#### Media
- Beej's Guide to Network Programming (Brian "Beej" Hall)

#### Method
Treat network sockets as I/O streams. Test your server manually using CLI tools like `curl` or `netcat`.

#### Practical Project
**Lightweight Static HTTP Server (`microhttpd`)**
Build a single-threaded HTTP web server in C. Open a TCP socket on port `8080`, bind it to local addresses, and listen for incoming client connections. Parse incoming HTTP `GET` request headers, read requested HTML or plain-text files from disk using POSIX system calls, format a valid HTTP/1.1 response header (`200 OK` or `404 Not Found`), and stream the file contents back to the client's browser.

## Future Modules
*(Reserved space for upcoming expansions: Computer Architecture & Assembly, Databases & Storage Engines, Compilers, Concurrency & Distributed Systems.)*
