# Preparation
  1. enable the hypervisor application in the VM
  2. more core cup and memory, the more performance you will have
  3. download two files from canvas
# Assignment 1
Your assignment is to create a Linux kernel module that will query various MSRs to determine virtualization features available in your CPU. This module will report (via the system message log) the features it discovers.

## Steps:

1. fork the https://github.com/torvalds/linux into your github and download your linux into your VM
2. install tools you need by "sudo apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf"
3. adding code to the file "cmpe283-1.c"
4. go to linux directory, check which linux is running by `uname -a`
5. call `make menuconfig` to create and interact with kernel configureation program
6. exit it and select "No"
7. call `cp /boot/config-5(chose the one from step 4) .config`
8. for some of the cases, may need check https://askubuntu.com/questions/1329538/compiling-the-kernel-5-11-11 and https://stackoverflow.com/questions/61657707/btf-tmp-vmlinux-btf-pahole-pahole-is-not-available
9. call `make oldconfig` to update the new things, hold the press till it is done
10. call `make prepare` to set the recomendation previously
11. go to main directory, call `make clean`
12. go to linux directroy, call `make -j number-of-cpu modules` to build modules(may install needed libraries based on command)
13. call `make -j number-of-cpu` to build real kernel
14. call `sudo make INSTALL_MOD_STRIP=1 modules_install` to package the kernel
15. call `sudo make install`
16. call `sudo reboot`
17. call `uname -a`, it should shows the new kernel is running
18. add `MODULE_LICENSE()` into the `cmpe283-1.c` file
19. call `make`
20. call `sudo insmod cmpe283-1.ko` to load the module
21. call `dmesg` to check the result for the assignment1
22. call `sudo rmmod cmpe283-1` to remove the module out of the kernel
23. move "MakeFile" and "cmpe283-1.c" to the file you build in linux directroy, then push to git

# Assignment2
- For CPUID leaf node %eax=0x4FFFFFFF:
  - Return the total number of exits (all types) in %eax
- For CPUID leaf node %eax=0x4FFFFFFE:
  - Return the high 32 bits of the total time spent processing all exits in %ebx
  - Return the low 32 bits of the total time spent processing all exits in %ecx
    - %ebx and %ecx return values are measured in processor cycles, across all VCPUs

## steps

1.Continue to use the Linux from Assignment1.

2.Install vm related packgages "sudo apt-get install qemu && sudo apt-get install qemu-kvm".

3.Go to */linux/arch/x86/kvm/cpuid.c* , add some code for the 0x4FFFFFFF and 0x4FFFFFFE, save it.

4.For Intel cpu, go to */linux/arch/x86/kvm/vmx/vmx.c*, add some code to the function `__vmx_handle_exit`, save it.

5.Call `sudo make -j cpu-core modules && sudo make INSTALL_MOD_STRIP=1 modules_install && sudo reboot` to rebuild, install and rebootkernel.

6.Search and download the corresponding os file.

7.You can use virt-manager to install and run your nested vm, steps [here](https://www.how2shout.com/how-to/qemu-ubuntu-tutorial.html).

8.I ran nested vm on qemu directly, stpes [here](https://techpiezo.com/linux/setup-virtual-machine-using-qemu-in-ubuntu/).

9.Inside of the nested vm, install cpuid package `sudo apt install cpuid`.

10.Get into your nested VM, run `cpuid -l 0x4fffffff` and `cpuid -l 0x4ffffffe` to test your code.

11.Back to Host, type `dmesg` to check the result.

# Assignment 3
- For CPUID leaf node %eax=0x4FFFFFFD:
  - Return the number of exits for the exit number provided (on input) in %ecx
    - This value should be returned in %eax
- For CPUID leaf node %eax=0x4FFFFFFC:
  - Return the time spent processing the exit number provided (on input) in %ecx
  - Return the high 32 bits of the total time spent for that exit in %ebx
    - Return the low 32 bits of the total time spent for that exit in %ecx

## steps
1.Continue to use the Linux from Assignment2.

3.Go to /linux/arch/x86/kvm/cpuid.c , add some code for the 0x4FFFFFFD and 0x4FFFFFFC (read VMX BASIC EXIT REASON ), save it.

4.For Intel cpu, go to /linux/arch/x86/kvm/vmx/vmx.c, add some code to the function **__vmx_handle_exit** and **vmx_handle_exit**, save it.

5.Call `sudo make -j cpu-core modules && sudo make -j cpu -core && sudo make modules_install && sudo make install && sudo reboot` to rebuild, install and reboot the kernel.

6.Get into your nested VM, run `cpuid -l 0x4ffffffc -s exit_nmber` and `cpuid -l 0x4ffffffd -s exit_number` to test your code.

7.Go to the Host terminal, type dmesg to check the kernal information.

# Qeustions:
- Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
  - Yes, the number of exits increast at a stable rate.
  - There are aproximately 1,800,000 exit.
- Of the exit types defined in the SDM, which are the most frequent? Least?
  - The most frequent exit type is External Interrupt.
  - The least frequent exit type is WBINVD.
## Result:

![Image of Result](https://github.com/Handsomenick1/linux/blob/master/cmpe283/Screen%20Shot%202021-11-03%20at%2010.06.20%20AM.png)

