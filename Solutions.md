# Problem 1

## Solution
A simple modification of the /boot.cfg file is required to change the timeout.

## Diffs
```
2c2
< timeout=7
---
> timeout=5
```

# Problem 2

## Solution
* Use grep to find which source file prints the prompt

    From /usr/src/sys/arch/i386/stand:
    ```
    grep -r ".*printf.*> .*" .
    ```

* Modify ./lib/menuutils.c where string is printed

* Recompile boot program code from same directory as above
    ```
    make && make install
    ```

* Take a hint from the lab and search for 'boot_monitor'
    1. Find this referenced in an 'updateboot.sh' script
    2. Run this program

## Diffs
```
$ diff -r /usr/src /usr/src-orig
diff -r /usr/src/sys/arch/i386/stand/lib/menuutils.c /usr/src-orig/sys/arch/i386/stand/lib/menuutils.c
73c73
<           printf(">> ");
---
>           printf("> ");
```
# Problem 3

## Solution
* Discover that 'getty' is the program that runs before the login shell

* Edit getty source code at /usr/src/commands/getty/getty.c

* Recompile from /usr/src/commands/getty
  ```
  make clean && make && make install
  ````

* Reboot

## Diffs
```
$ diff -r /usr/src/commands/getty/getty.c /usr/src-orig/commands/getty/getty.c
91c91
<   static char *def_banner[] = { "%s Lab5 RELEASE %r VERSION %v  (%t)\n\n%n login: ", 0 };
---
>   static char *def_banner[] = { "%s  Release %r Version %v  (%t)\n\n%n login: ", 0 };
```

# Problem 4

## Solution
* Modify the ttys file to include an entry for a new console

* Change 'NR_CONS' in /usr/src/include/minix/config.h from 4 to 5 

* Navigate to /dev and run MAKEDEV
  ```
  $ cd /dev
  $ MAKEDEV ttyc4
  ```
  
* Make new image and install to hard disk
  ```
  $ cd /usr/src/releasetools
  $ make hdboot
  ```
  
## Diffs
```
$ diff /etc/ttys /etc/ttys.bak
10d9
< ttyc4     getty       minix   on secure
```

```
$ diff config.h config.h.bak
52c52
< #define NR_CONS   4   /* # system consoles (1 to 8) */
---
> #define NR_CONS   5   /* # system consoles (1 to 8) */
```
