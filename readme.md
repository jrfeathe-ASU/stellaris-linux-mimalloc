This repo describes how to use mimalloc with 1GiB hugepages when running Stellaris on Linux. The process was tested on Linux Mint 21.1 Cinnamon with 32GB of RAM. The process was described as "trivial" by this poster, yet I found it to be quite confusing and difficult. https://forum.paradoxplaza.com/forum/threads/high-level-wishlist-suggestions-to-improve-game-performance.1507394/

****PROCESS****:
1. Install mimalloc from the repo https://github.com/microsoft/mimalloc
  ```
  > sudo apt install cmake gcc g++ git
  > git clone https://github.com/microsoft/mimalloc.git  
  > cd mimalloc
  > mkdir -p out/release
  > cd out/release
  > cmake ../..
  > make
  > sudo make install
  ```
  Check that the file was installed:
  ```
  > ls /usr/local/lib/libmimalloc.so
  ```
2. Enable eight 1GiB Hugepages (MAY BE TOO MUCH ON 16GB RAM):
  ```
  > sudo sh -c "echo 8 > /sys/kernel/mm/hugepages/hugepages-1048576kB/nr_hugepages"
  ```
  Close terminal and re-open. Check to see how many pages are open. HugePages_Total should be 8. Hugepagesize should be 1048576 kB. 
  ```
  > cat /proc/meminfo | grep Huge
  ```

Grub update to enable large pages
Backup grub.cfg
/env/default/grub
GRUB_CMDLINE_LINUX="default_hugepagesz=1G hugepagesz=1G hugepages=8"
update grub
https://www.intel.com/content/www/us/en/docs/programmable/683840/1-2-1/enabling-hugepages.html


Create the file stellaris_mimalloc in the Stellaris folder.
```
#!/bin/bash
cd "$(dirname "$0")"
env MIMALLOC_VERBOSE=1 MIMALLOC_RESERVE_HUGE_OS_PAGES=6 LD_PRELOAD=$(find /usr -name "libmimalloc.so" | head -n 1) ./stellaris &> ~/stellaris_mimalloc.log
```
Mark as exec

Launch Steam
Launch Stellaris but do not click play in PDX Launcher

```
./stellaris_malloc
```

UNFINISHED... TO BE CONTINUED.

RESOURCES:

https://github.com/microsoft/mimalloc
