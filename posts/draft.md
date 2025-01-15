---
aliases:
- /OS/WriteUps/XV6-Lab4-2024
categories:
- Write-ups
date: '2025-01-14'
description: Answer for lab4 2024
image: images/xv6_mit.png
layout: post
title: "DRAFT"
toc: true
draft: true
---
## Lab : traps

@author : FlyingPig

@email : zhongyinmin@pku.edu.cn

### 1. Overview

--- 

xv6 implements system calls through the trap mechanism. This lab will allow students to first familiarize themselves with the user stack and then implement a trap handler.



### 2. Code Implementation

--- 

#### 2.1 Backtrace

--- 

When debugging, we hope to know which functions are called above the stack when an error occurs in the code.

![image-20210110212219277](/Users/apple/Library/Application Support/typora-user-images/image-20210110212219277.png)

The above is a screenshot from a class slide, which shows the structure of the RISC-V stack frame very well, from which backtrace can be easily implemented

```c 
void 
backtrace(void) 
{ 
  uint64 cur_fp = r_fp(); 
  while(cur_fp != PGROUNDDOWN(cur_fp)) 
  { 
    printf("%p\n", *(uint64 *)(cur_fp - 8)); 
    cur_fp = *(uint64 *)(cur_fp - 16); 
  } 
} 
``` 

It should be noted that xv6 only allocates one page (4KB) as the user stack to each user process, so it is possible to determine whether the stack head has been reached by checking whether fp has reached the beginning of the page.

#### 2.2 Alarm

In this exercise, you will add a feature to xv6 that will periodically sound an alarm to a process as it uses CPU time. This feature can be useful for processes that want to limit how much CPU time they use, or for processes that want to take some periodic action. More generally, you will implement a primitive form of user-level interrupt/fault handler; for example, you could use something similar to handle page faults in your application.

To achieve this goal, you need to add a sigalarm(interval, handler) system call, which will cause the process that calls this system call to automatically call the handler function every interval ticks of the CPU.

First, you need to add corresponding variables to the process proc data structure:

```c 
/ ======== alarm solution ========= 
  uint64 handler; 
  int alarm_interval; 
  int passed_ticks; 
  int allow_entrance_handler; 
  uint64 saved_epc; // saved user program counter 
  uint64 saved_ra; uint64 
  saved_sp; 
  uint64 saved_gp; 
  uint64 saved_tp 
  ; uint64 saved_t0; 
  uint64 saved_t1; uint64 saved_t2 
  ; uint64 saved_t3 
  ; uint64 saved_t4; uint64 saved_t5; uint64 saved_t6; 
  uint64 saved_a0 
  ; 
  uint64 saved_a1; uint64 saved_a2; 
  uint64 
  saved_a3 
  ; 
  uint64 
  saved_a4; 
  uint64 saved_a5; uint64 saved_a6; uint64 
  saved_a7 
  ; 
  uint64 saved_s0; 
  uint64 saved_s1; 
  uint64 
  saved_s2; uint64 saved_s3; uint64 saved_s4 
  ; 
  uint64 saved_s5; uint64 
  saved_s6; uint64 
  saved_s7; 
  uint64 saved_s8; 
  uint64 saved_s9; 
  uint64 saved_s10; 
  uint64 saved_s11; 
  // ================================= 
``` 
Then implement the sigalarm system call, which will assign the attributes of the corresponding process.
```c 
uint64 
sys_sigalarm(void) 
{ 
  int interval; 
  uint64 handler; 
  if (argint(0, &interval) < 0) 
    return -1; 
  if (argaddr(1, &handler) < 0)


    return -1; 
  myproc()->alarm_interval = interval; 
  myproc()->handler = handler; 
  return 0 ; 
} 
``` 

If you want to automatically load xv6 with an exception, trap the default settings:

```c 
// give up the CPU if this is a timer interrupt. 
  if(which_dev == 2) 
  { 
    // ======= alarm solution ======== 
    p->passed_ticks += 1; 
    if (p->passed_ticks == p->alarm_interval) 
    { 
      if (p->allow_entrance_handler) 
      { 
        // avoid re-entrant 
        p->allow_entrance_handler = 0; 

        // save all the needed registers 
        p->saved_epc = p->trapframe->epc; // saved user program counter 
        p->saved_ra = p->trapframe->ra; 
        p->saved_sp = p->trapframe->sp; 
        p->saved_gp = p->trapframe->gp; 
        p->saved_tp = p->trapframe->tp; 
        p->saved_t0 = p->trapframe->t0; 
        p->saved_t1 = p->trapframe->t1; 
        p->saved_t2 = p->trapframe->t2; 
        p->saved_t3 = p->trapframe->t3; 
        p->saved_t4 = p->trapframe->t4; 
        p->saved_t5 = p->trapframe->t5; 
        p->saved_t6 = p->trapframe->t6; 
        p->saved_a0 = p->trapframe->a0; 
        p->saved_a1 = p->trapframe->a1; 
        p->saved_a2 = p->trapframe->a2; 
        p->saved_a3 = p->trapframe->a3; 
        p->saved_a4 = p->trapframe->a4; 
        p->saved_a5 = p->trapframe->a5; 
        p->saved_a6 = p->trapframe->a6; 
        p->saved_a7 = p->trapframe->a7; 
        p->saved_s0 = p->trapframe->s0; 
        p->saved_s1 = p->trapframe->s1; 
        p->saved_s2 = p->trapframe->s2; 
        p->saved_s3 = p->trapframe->s3; 
        p->saved_s4 = p->trapframe->s4; 
        p->saved_s5 = p->trapframe->s5; 
        p->saved_s6 = p->trapframe->s6; 
        p->saved_s7 = p->trapframe->s7; 
        p->saved_s8 = p->trapframe->s8; 
        p->saved_s9 = p->trapframe->s9; 
        p->saved_s10 = p->trapframe->s10; 
        p->saved_s11 = p->trapframe->s11; 
        // if the process returns the user code,
        // jump to the handler code first
        p->trapframe->epc = p->handler;

        // re-arm the alarm
        p->passed_ticks = 0;
      } else {
        // can not enter handler code
        p->passed_ticks -= 1;
      }
    }
    // ==================================================
    yield();
  } 
``` 

Finally, since the handler function will call the sigreturn system call by default at the end, we only need to restore the original register values ​​in this system call.

```c 

uint64 
sys_sigreturn(void) 
{ 
  // restore all the saved registers 
  struct proc *p = myproc(); 
  p->trapframe->epc = p->saved_epc; 
  p->trapframe->ra = p->saved_ra; 
  p->trapframe->sp = p->saved_sp; 
  p->trapframe->gp = p->saved_gp; 
  p->trapframe->tp = p->saved_tp; 
  p->trapframe->a0 = p->saved_a0; 
  p->trapframe->a1 = p->saved_a1; 
  p->trapframe->a2 = p->saved_a2; 
  p->trapframe->a3 = p->saved_a3; 
  p->trapframe->a4 = p->saved_a4; 
  p->trapframe->a5 = p->saved_a5; 
  p->trapframe->a6 = p->saved_a6; 
  p->trapframe->a7 = p->saved_a7; 
  p->trapframe->t0 = p->saved_t0; 
  p->trapframe->t1 = p->saved_t1; 
  p->trapframe->t2 = p->saved_t2; 
  p->trapframe->t3 = p->saved_t3; 
  p->trapframe->t4 = p->saved_t4; 
  p->trapframe->t5 = p->saved_t5; 
  p->trapframe->t6 = p-> 
  saved_t6; p->trapframe->s0 = p->saved_s0; 
  p->trapframe->s1 = p->saved_s1; 
  p->trapframe->s2 = p->saved_s2; 
  p->trapframe->s3 = p->saved_s3; 
  p->trapframe->s4 = p->saved_s4; 
  p->trapframe->s5 = p->saved_s5; 
  p->trapframe->s6 = p->saved_s6; 
  p->trapframe->s7 = p->saved_s7; 
  p->trapframe- 
  >s8 = p->saved_s8; 
  p->trapframe->s9 = p->saved_s9; p->trapframe->s10 = p->saved_s10; 
  p->trapframe->s11 = p->saved_s11; 

  myproc()->allow_entrance_handler = 1; 
  return 0; 
} 
```