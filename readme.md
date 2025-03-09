This repo describes how to use mimalloc with 1GiB hugepages when running Stellaris on Linux. The process was tested on Linux Mint 21.1 Cinnamon with 32GB of RAM. The process was described as "trivial" by this poster, yet I found it to be quite confusing and difficult. https://forum.paradoxplaza.com/forum/threads/high-level-wishlist-suggestions-to-improve-game-performance.1507394/

****PROCESS****:
1. Install mimalloc from the repo https://github.com/microsoft/mimalloc
  > sudo apt install cmake gcc g++ git
  > git clone https://github.com/microsoft/mimalloc.git  
  > cd mimalloc
  > mkdir -p out/release
  > cd out/release
  > cmake ../..
  > make
  > sudo make install
  Check that the file was installed:
  > ls /usr/local/lib/libmimalloc.so
2. Enable eight 1GiB Hugepages (MAY BE TOO MUCH ON 16GB RAM):
  > sudo sh -c "echo 8 > /sys/kernel/mm/hugepages/hugepages-1048576kB/nr_hugepages"
  Close terminal and re-open.
  > cat /proc/meminfo | grep Huge

RESOURCES:

https://github.com/microsoft/mimalloc
