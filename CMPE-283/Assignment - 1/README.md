# CMPE 283: Virtualization Technologies 
# Assignment 1: Discovering VMX Features
## Team Members: 
* Kiran Bala Devineni (SJSU ID: 015218866)
* Venkat Mannam (SJSU ID: 015263326)

## Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.

### Kiran Bala Devineni (SJSU ID: 015218866)

* Setup the environment in macOS using VMware Fusion 12 and then installed Linux Ubuntu.
* Downloaded and built the Linux Kernel modules and associated libraries to create a local copy of Linux Kernel.
* Discussed and researched about MSRs to be read in the SDM.
* Modified the cmpe283-1.c code by adding the custom logic to enable our system to read and give output for capabilities of the various MSRs. 
* Contribution also includes MSR code for controls- Primary and Secondary Procbased controls and discovering the VMX Features present in my processor (Intel) by writing a Linux kernel module that queries these features.
* Tested and verified the proper working of the functionality of code by comparing it with the sample output given to us. 
* Simulating the answers for the questions in the README.md file.

### Venkat Mannam (SJSU ID: 015263326)

* Setup the environment in macOS using VMware Fusion 12 and downloaded the Linux ISO file. 
* Built a VM successfully in the first attempt by allocating 150GB storage and 8GB RAM to it. 
* Tested the machine to check its capability for VMX virtualization and feature recognition. 
* Researched and discussed MSRs to be read in the SDM and contributed to the writing and execution of the code.
* Contribution also includes MSR code for controls- Entry and Exit controls and determining the availability of secondary procbased controls.
* Staged and committed the cmpe283-1.c code file and Makefile after inserting the module and printing out the buffer from the Kernel. 
* Generated a comprehensive diff file after committing the changes to the repository. 

## Q2. Describe in detail the steps you used to complete the assignment. 

This assignment focuses on the creation of a Linux kernel module to query various MSRs to determine the virtualization features present in the CPU. The module will then report the features it discovers as a system message log.

**Prerequisite:** A machine capable of running Linux, with VMX virtualization features exposed.

#### Steps followed to complete the assignment:
1. To begin, install VMware Fusion and use the Ubuntu ISO to create a virtual machine. Then enable virtualization in the VM.<br />

   https://ubuntu.com/download/desktop

2. Fork the Linux repository:<br />

   https://github.com/torvalds/linux

3. Install git:<br />
```
   sudo apt-get install git
```
4. Using the command below, clone the kernel sources from the master linux git repository.<br />
```
   git clone https://github.com/venkatmannam3/linux.git 
```
5. Install the required packages for building a linux kernel:<br />
```
   sudo apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
``` 
6. Check the kernel version you are building and copy the appropriate config file into the cloned linux folder:
```
   uname -a
```
```
   cd linux
```
```
   cp /boot/config-(linux kernel version) .config
```
7. Make the oldconfig file to set the required configuration for building the kernel and just hit enter for every question:
```
   make oldconfig
```
8. Run the following command to prepare to build the kernel:
```
   make prepare
```
9. Install all the required modules for the kernel using the command:
```
   make modules
```
10. Now build the kernel using the make command:
```
   make
```
11. Copy all the modules installed in the 9th step into the appropriate folder by using:
```
   sudo make INSTALL_MOD_STRIP=1 modules_install
```
12. Build the kernel using the following command:
```
   make install
```
13. Reboot the Ubuntu system to load into the latest kernel build
```
   sudo reboot
```
14. In the previously cloned "linux" source folder, create a new directory called "cmpe283" using the command below.<br />
```
   mkdir cmpe283
```  
15. To go to the cmpe283 directory, use the command:<br />
```
   cd cmpe283
```   
16. To the cmpe283 directory, copy the "Makefile" and cmpe283-1.c that are provided.
17. Add the following required licenses into the .c file that we copied:
```
   MODULE_LICENSE("GPL_v2");
```
18. Run the make function to run the Makefile
```
   make
```
19. The functionality for querying all other MSRs is described further below.

    * By referring to SDM, we created different structures with name (description) and bit positions for entry, exit, pinbased, procbased, secondary procbased controls.

    * To detect and print VMX capabilities of CPU, the function report_capability( ) is called with appropriate parameters passed in order to print pinbased, procbased, entry, and exit controls.
   
20. Load and unload the specific kernel module into the kernel using the following commands:<br />

    * When a module is inserted into the kernel, the module_init macro will be invoked, which will call the function init_module. 
    
    * Similarly, when the module is removed with rmmod, module_exit macro will be invoked, which will call the cleanup_module.
```
    sudo insmod ./cmpe283-1.ko
```
```
    sudo rmmod ./cmpe283-1.ko
```    
21. The VMX features get logged in the kernel log and using the below command, we can verify the message buffer/output from the kernel in the system message log:<br />
```
    dmesg 
```

## Output:

![image](https://user-images.githubusercontent.com/78829969/141673800-6a7fc056-a414-4d86-8072-a9df8d3d981e.png)

![image](https://user-images.githubusercontent.com/78829969/141673814-eb202ed9-7de0-4792-8b49-33700f56b184.png)

![image](https://user-images.githubusercontent.com/78829969/141673820-708196c8-500d-4779-b22a-983634e18d71.png)














    
    
