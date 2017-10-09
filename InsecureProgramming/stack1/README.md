## Source code
```c
/* stack1-stdin.c                               *
 * specially crafted to feed your brain by gera */

#include <stdio.h>

int main() {
	int cookie;
	char buf[80];

	printf("buf: %08x cookie: %08x\n", &buf, &cookie);
	gets(buf);

	if (cookie == 0x41424344)
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
gcc version 6.1.1 20160802 (Debian 6.1.1-11)
```
Compile the code with gcc without protections
If you fall into a similar error ```/usr/include/features.h:364:25: fatal error: sys/cdefs.h: No such file or directory```
```bash
sudo apt-get install g++-multilib
```
```bash
gcc -m32 -z execstack -fno-stack-protector -o stack1 stack1.c
```

