# CS2107 Lecture 10

## Memory after reset
The memory of the computer retains the same values as it had before the reset. It may be possible to view the contents of the memory by booting a different OS.

Similar for swap space.

## `malloc` and `free` for sensitive data
Passwords which are saved to memory using `malloc()` needs to be cleared using `bzero()` before doing `free()`.

## Page fault hack
By putting a string at page boundaries, when the string is accessed by programs used to authenticate passwords, page faults occur when page boundaries are crossed. By observing the page faults, individual characters can be eliminated, to reduce the number of tries to a small amount.

## Race conditions
A race condition is an undesirable situation that occurs when a device or system attempts to perform two or more operations at the same time, but because of the nature of the device or system, the operations must be done in the proper sequence to be done correctly. It can lead to privilege escalation.

Important to check **critical sections** to ensure time-to-check-to-time-of-use (TOCTTOU) vulnerabilities are not present.