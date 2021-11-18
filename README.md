## Preparation
  1. enable the hypervisor application in the VM
  2. more core cup and memory, the more performance you will have
  3. download two files from canvas
----------------------------------------------------------------------------------------------
## Result:

![Image of Result](https://github.com/Handsomenick1/linux/blob/master/cmpe283/Screen%20Shot%202021-11-03%20at%2010.06.20%20AM.png)

## Steps:

1. fork the https://github.com/torvalds/linux into your github and download your linux into your VM

2. install tools you need by "sudo apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf"

3. adding code to the file "cmpe283-1.c"

4. go to linux directory, check which linux is running by "uname -a"

5. call "make menuconfig" to create and interact with kernel configureation program

6. exit it and select "No"

7. call "cp /boot/config-5(chose the one from step 4) .config

8. for some of the cases, may need to call "scripts/config --disable SYSTEM_REVOCATION_KEYS"

9. call "make oldconfig" to update the new things, hold the press till it is done

10. call "make prepare" to set the recomendation previously 

11. go to main directory, call "make clean"

14. go to linux directroy, call "make -j number-of-cpu modules" to build modules(may install needed libraries based on command)

15. call "make -j number-of-cpu" to build real kernel 

16. call "sudo make INSTALL_MOD_STRIP=1 modules_install" to package the kernel 

17. call "sudo make install"

18. call "sudo reboot"

19. call "uname -a", it should shows the new kernel is running

20. add "MODULE_LICENSE()" into the "cmpe283-1.c" file

21. call "make"

22. call "sudo insmod cmpe283-1.ko" to load the module

23. call "dmesg" to check the result for the assignment1

24. call "sudo rmmod cmpe283-1" to remove the module out of the kernel

25. move "MakeFile" and "cmpe283-1.c" to the file you build in linux directroy, then push to git



