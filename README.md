# learndpdk
experimenting with dpdk

Started on with the only document from http://doc.dpdk.org/
Will be using a centos 7+ distribution for the experimentations

PreReq
------
Kernel version >= 3.16
<pre>
uname -r
</pre>
glibc >= 2.7
<pre>
ldd --version
</pre>
HUGETLBFS option must be enabled in the running kernel

Reserving Hugepages for DPDK Use - 1024 pages of 2MB page size
<pre>
echo 1024 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages
</pre>

For NUMA nodes see below
<pre>
echo 1024 > /sys/devices/system/node/node0/hugepages/hugepages-2048kB/nr_hugepages
echo 1024 > /sys/devices/system/node/node1/hugepages/hugepages-2048kB/nr_hugepages
</pre>

Tool for managing hugepages
<pre>
dpdk-hugepages.py
</pre>
Kernel versions that does not allow reserving hugepages at run time requires reservation during boot by passing 
additional parameters to kernel command

<pre>
hugepages=1024                <<== reserves 1024 pages of 2 MB
hugepagesz=1G hugepages=4     <<== reserves 4 pages of 1G
default_hugepagesz=1G         <<== changes the default allocation to 1G
</pre>

DPDK is able to use hugepages without any configuration by using “in-memory” mode.
If secondary process support is required, mount points for hugepages need to be created.

Default mount point for higepages of defaultsize is at
<pre>
/dev/hugepages
</pre>
To make the hugepages of size 1GB available for DPDK use, following steps must be performed:
<pre>
mkdir /mnt/huge
mount -t hugetlbfs pagesize=1GB /mnt/huge
</pre>
The mount point can be made permanent across reboots, by adding the following line to the /etc/fstab file:
<pre>
nodev /mnt/huge hugetlbfs pagesize=1GB 0 0
</pre>
