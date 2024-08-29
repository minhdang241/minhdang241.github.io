---
title: Process 
markmap:
  colorFreezeLevel: 2
  maxWidth: 350
---
## Definition
- A running program. 
- Each process has a unique name known as `process ID`.

## States
- **Running**: It is running.
- **Ready**: It is ready to run, but the OS decides to not run it. 
- **Blocked**: It is performing I/O.

## APIs provided by OS 
- [`fork()`](https://man7.org/linux/man-pages/man3/fork.3p.html): Create a new process. The caller is the **parent**, the created one is the **child**.
- [`wait()`](https://man7.org/linux/man-pages/man3/wait.3p.html): Wait for the child process to complete execution. 
- [`exec()`](https://man7.org/linux/man-pages/man3/exec.3p.html): Replace the current program with a new program yet using the same process.
- ...