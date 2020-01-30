# linetrace-cmd-record

Wrapper around [`trace-cmd record`](
https://git.kernel.org/pub/scm/linux/kernel/git/rostedt/trace-cmd.git), which allows recording execution of all source
lines in specified kernel functions by using kernel debuginfo and kprobes.

# Installation

Fedora:

```
$ sudo dnf install -y pypy3
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ sudo pypy3 get-pip.py --user
$ sudo pypy3 -m pip install --upgrade --user -r requirements.txt
```

# Usage

```
$ sudo pypy3 linetrace-cmd-record [-L func] [trace-cmd record args ...]
```

Example - debugging rootless podman:

```
$ sudo pypy3 linetrace-cmd-record -L map_write -p function_graph -g proc_gid_map_write sudo -u $USER podman build $HOME/foo -t foo
$ trace-cmd report | grep -v -e ^CPU -e ^cpus= | head -n 20
           <...>-1571473 [005] 1198357.632237: funcgraph_entry:                   |  proc_gid_map_write() {
           <...>-1571473 [005] 1198357.632242: user_namespace_c_L849_C1_0x226a68: (437aea68)
           <...>-1571473 [005] 1198357.632245: funcgraph_entry:                   |    map_write() {
           <...>-1571473 [005] 1198357.632247: user_namespace_c_L851_C2_0x226a6e: (437aea6e)
           <...>-1571473 [005] 1198357.632248: user_namespace_c_L850_C2_0x226a6e: (437aea6e)
           <...>-1571473 [005] 1198357.632250: user_namespace_c_L859_C2_0x226aae: (437aeaae)
           <...>-1571473 [005] 1198357.632251: user_namespace_c_L856_C2_0x226aae: (437aeaae)
           <...>-1571473 [005] 1198357.632251: user_namespace_c_L855_C2_0x226aae: (437aeaae)
           <...>-1571473 [005] 1198357.632252: user_namespace_c_L854_C2_0x226aae: (437aeaae)
           <...>-1571473 [005] 1198357.632252: user_namespace_c_L853_C2_0x226aae: (437aeaae)
           <...>-1571473 [005] 1198357.632253: user_namespace_c_L852_C2_0x226aae: (437aeaae)
           <...>-1571473 [005] 1198357.632255: user_namespace_c_L863_C2_0x226ac6: (437aeac6)
           <...>-1571473 [005] 1198357.632255: funcgraph_entry:                   |      memdup_user_nul() {
           <...>-1571473 [005] 1198357.632255: funcgraph_entry:                   |        __kmalloc_track_caller() {
           <...>-1571473 [005] 1198357.632255: funcgraph_entry:        0.091 us   |          kmalloc_slab();
           <...>-1571473 [005] 1198357.632256: funcgraph_entry:        0.105 us   |          should_failslab();
           <...>-1571473 [005] 1198357.632256: funcgraph_entry:        0.086 us   |          memcg_kmem_put_cache();
           <...>-1571473 [005] 1198357.632256: funcgraph_exit:         0.690 us   |        }
           <...>-1571473 [005] 1198357.632256: funcgraph_entry:        0.179 us   |        raw_copy_from_user();
           <...>-1571473 [005] 1198357.632256: funcgraph_exit:         1.112 us   |      }
```
