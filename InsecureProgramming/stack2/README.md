## Source code
```c
/* stack2-stdin.c                               *
 * specially crafted to feed your brain by gera */

#include <stdio.h>

int main() {
	int cookie;
	char buf[80];

	printf("buf: %08x cookie: %08x\n", &buf, &cookie);
	gets(buf);

	if (cookie == 0x01020305)
  		printf("you win!\n");
}
```

## Setup
Check if ASLR is disabled
```bash
sysctl kernel.randomize_va_space
kernel.randomize_va_space = 0
```

Check gcc version
```bash 
gcc -v 
gcc version 7.2.0 (Debian 7.2.0-8)
```

Compile the code with gcc without protections for 32-bits
```bash
gcc -m32 -z execstack -fno-stack-protector -o stack2 stack2.c
```

If you fall into a similar error ```/usr/include/features.h:364:25: fatal error: sys/cdefs.h: No such file or directory``` run
```bash
sudo apt-get install g++-multilib
```

## Exploitation

This exercise is exactly the same than the [stack1](https://github.com/xufuou/learning-reverse/tree/master/InsecureProgramming/stack1) but with the change in the comparison value for cookie. Since the new value is 0x41424344 and does not represent any printable value in ASCII unlike the example stack1, we have to pass the byte sequence as it is with(don't forget to reverse) with the help of python for example.

![alt text](https://imgur.com/fTVF5Dc.png)

