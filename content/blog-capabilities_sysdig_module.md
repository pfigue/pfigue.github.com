title: Containers and Capabilities
tags: containers, docker, sysdig, capabilities
date: 2015-11-03
slug: container-capabilities
authors: pfigue
category: containers
status: published


So, I was inside of a container and I wanted to use [sysdig](http://www.sysdig.org/). Sysdig needs a `sysdig-probe` module inside the kernel, which will trap all system calls and log them into a buffer.

If I had full root access to the host, I could `modprobe` the module myself, but I haven't, as I was getting:

    # modprobe sysdig-probe
    FATAL: Error inserting sysdig_probe (/lib/modules/3.13.0-37-generic/updates/dkms/sysdig-probe.ko): Operation not permitted
    
That was strange to me as I was root.
    
# What is controlling this behaviour?
Well, I was researching and I found the `man 7 capabilities` page. It seems there are two capabilities (CAP_SYS_MODULE and CAP_SYS_ADMIN) which seems to allow a thread to perform *insmod/rmmod* operations.

*Note: somewhere I read it is CAP_SYS_ADMIN, and somewhere else I saw it is actually CAP_SYS_MODULE. Right now, I don't know if there are two, or it depends on the kernel version/config.*

# Which capabilities has my container?
From inside my container I ran:

    # cat /proc/self/status
    [...]
    CapInh: 0000000000000000
    CapPrm: 0000001dbdbeffff
    CapEff: 0000001dbdbeffff
    CapBnd: 0000001dbdbeffff
    [...]

And I saw those four CapXXX parameters, described a little bit in [Table 1-2](https://www.kernel.org/doc/Documentation/filesystems/proc.txt).

I wrote a [little C program](https://gist.github.com/pfigue/3044c6a38be254645ca0) to decipher that hexadecimal value 0x0000001dbdbeffff:

    $ ./cap1 0000001dbdbeffff                                                                                                                                                                                    
    capab.: 0000001dbdbeffff
    CAP_CHMOD(0) is enabled.
    CAP_DAC_OVERRIDE(1) is enabled.
    CAP_DAC_READ_SEARCH(2) is enabled.
    CAP_FOWNER(3) is enabled.
    CAP_FSETID(4) is enabled.
    CAP_KILL(5) is enabled.
    CAP_SETGID(6) is enabled.
    CAP_SETUID(7) is enabled.
    CAP_SETPCAP(8) is enabled.
    CAP_LINUX_IMMUTABLE(9) is enabled.
    CAP_NET_BIND_SERVICE(10) is enabled.
    CAP_NET_BROADCAST(11) is enabled.
    CAP_NET_ADMIN(12) is enabled.
    CAP_NET_RAW(13) is enabled.
    CAP_IPC_LOCK(14) is enabled.
    CAP_IPC_OWNER(15) is enabled.
    CAP_SYS_MODULE(16) is disabled.
    CAP_SYS_RAWIO(17) is enabled.
    CAP_SYS_CHROOT(18) is enabled.
    CAP_SYS_PTRACE(19) is enabled.
    CAP_SYS_PACCT(20) is enabled.
    CAP_SYS_ADMIN(21) is enabled.
    CAP_SYS_BOOT(22) is disabled.
    CAP_SYS_NICE(23) is enabled.
    CAP_SYS_RESOURCE(24) is enabled.
    CAP_SYS_TIME(25) is disabled.
    CAP_SYS_TTY_CONFIG(26) is enabled.
    CAP_MKNOD(27) is enabled.
    CAP_LEASE(28) is enabled.
    CAP_AUDIT_WRITE(29) is enabled.
    CAP_AUDIT_CONTROL(30) is disabled.
    CAP_SETFCAP(31) is enabled.
    
To sum up: *CAP_SYS_MODULE*, *CAP_SYS_BOOT*, *CAP_SYS_TIME*, *CAP_AUDIT_CONTROL* are disabled and *CAP_SYS_ADMIN* is enabled.

What is preventing me to insert that module is that disabled *CAP_SYS_MODULE*. How to enable it from inside the container? I don't know yet how to achieve that.

# Refs:

  1. [Kerneldocs - The /proc filesystem](https://www.kernel.org/doc/Documentation/filesystems/proc.txt)
  2. [Decoding the capabilities mask.](https://gist.github.com/pfigue/3044c6a38be254645ca0)
  3. [sysdig](http://www.sysdig.org/)
