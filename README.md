# linetrace-cmd-record

Wrapper around [`trace-cmd record`](
https://git.kernel.org/pub/scm/linux/kernel/git/rostedt/trace-cmd.git), which allows recording execution of all source
lines in specified kernel functions by using kernel debuginfo and kprobes.

# Usage

```
$ sudo pypy3 linetrace-cmd-record [-L func] [trace-cmd record args ...]
```
