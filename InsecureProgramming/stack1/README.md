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
gcc version 7.2.0 (Debian 7.2.0-8)
```

Compile the code with gcc without protections for 32-bits
```bash
gcc -m32 -z execstack -fno-stack-protector -o stack1 stack1.c
```

If you fall into a similar error ```/usr/include/features.h:364:25: fatal error: sys/cdefs.h: No such file or directory``` run
```bash
sudo apt-get install g++-multilib
```

## Exploitation

Looking the source code this exercise is quite straightforward we have a int variable named cookie and a char array of lenght 80 named buf that uses the [dangerous gets](https://stackoverflow.com/questions/1694036/why-is-the-gets-function-so-dangerous-that-it-should-not-be-used) function to read from stdin to buf. This snippet is cleary vulnerable to a buffer overflow attack.

Let's run the program:

![alt text](https://i.imgur.com/lpYD3tj.png)

Since the stack grows top down we can get the offset from the buf and cookie by subtracting ffffd65c to ffffd6ac which gives exactly 80 bytes as we expected. We can now overflow the buf to overwrite the value of cookie. Since the cookie value has to be 0x41424344 which is ABCD. Don't forget that the binary was compiled for 32-bit and thus the program memory adresses are represented in [little endian](https://www.cs.umd.edu/class/sum2003/cmsc311/Notes/Data/endian.html), we have to invert the payload ABCD to DCBA.

![alt text](https://imgur.com/gRCTAWK.png)


