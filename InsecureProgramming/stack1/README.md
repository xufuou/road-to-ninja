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
0. Check gcc version
```bash gcc -v 
gcc version 6.1.1 20160802 (Debian 6.1.1-11)
```
1. Compile the code with gcc
