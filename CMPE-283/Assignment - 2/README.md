# CMPE 283: Virtualization Technologies
# Assignment 2: Instrumentation via hypercall
The assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.

* For CPUID leaf node %eax=0x4FFFFFFF:<br />
  ◦ Return the total number of exits (all types) in %eax
* For CPUID leaf node %eax=0x4FFFFFFE:<br />
  ◦ Return the high 32 bits of the total time spent processing all exits in %ebx<br />
  ◦ Return the low 32 bits of the total time spent processing all exits in %ecx<br />
      %ebx and %ecx return values are measured in processor cycles, across all VCPUs

## Team Members: 
* Kiran Bala Devineni (SJSU ID: 015218866)
* Venkat Mannam (SJSU ID: 015263326)

## Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.

### Kiran Bala Devineni (SJSU ID: 015218866)

* Setup the environment in macOS using VMware Fusion 12 and then installed Linux Ubuntu.
* Downloaded and built the Linux Kernel modules and associated libraries to create a local copy of Linux Kernel.
* Researched and discussed the exits and interrupts by referring to the Intel SDM.
* Contribution also includes writing code for returning the total number of exits and the total time spent processing all exits. 
* Modified the function kvm_emulate_cpuid in the following file: linux/arch/x86/kvm -> cpuid.c and vmx_handle_exit in the following file: linux/arch/x86/kvm/vmx -> vmx.c 
* Simulating the answers for the questions in the README.md file.

### Venkat Mannam (SJSU ID: 015263326)

* Setup the environment in macOS using VMware Fusion 12 and downloaded the Linux ISO file. 
* Built a VM successfully in the first attempt by allocating 150GB storage and 8GB RAM to it.
* Built another VM inside using the Ubuntu ISO file.
* Tested the machine to check its capability for VMX virtualization. 
* Researched and discussed MSRs to be read in the SDM and contributed to the testing of the code.
* Contribution also includes writing code for the test program that exercises the functionality in the hypervisor's modification.

## Q2. Describe in detail the steps you used to complete the assignment. 

**Prerequisite:** A working assignment 1 configuration.

#### Steps followed to complete the assignment:

Part 1: How we built the kernel

1. We followed the steps to install the VM, then Ubuntu on our Intel-based MacBook pro. Installed ISO - Ubuntu, allocated disk space of 150GB.

2. We have cloned the Linux github repository, using following command: 
```
git clone https://github.com/torvalds/linux.git
```
3. We followed the insructions given in the Assignment pdf to build kernel.

4. Check the Linux Version (old):<br />
```
uname -a
```
5. Build the kernel by running the below command:
```
sudo apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev
```
6. Check the kernel version and copy the appropriate config file into the cloned linux folder: 
```
uname -r 
```
```
cd linux
```
```
cp /boot/config-`uname -r` .config
```
7. Make the oldconfig file to set the required configuration for building the kernel and just hit enter for every question:
``` 
make oldconfig
```
8. Run the following instruction in "Linux" folder:
```
make -j 2 modules && make -j 2 && sudo make modules_install  && sudo make install
```
9. Then reboot the Ubuntu machine: 
```
sudo reboot
```
10. Verify that the newer kernel is being used after reboot.
```
uname -a
```
After building kernel:<br />
devki@ubuntu:~$ uname -a<br />
Linux ubuntu 5.12.0-rc7+ #1 SMP Sun Nov 21 13:34:02 PDT 2021 x86_64 x86_64 x86_64 GNU/Linux

### Implement the Assignment Functionalities:

We edited cpuid.c and vmx.c for implementing code for calculating total number of exits and the total time spent processing all exits.

We modified function kvm_emulate_cpuid in the following file: linux/arch/x86/kvm/cupid.c, 
and vmx_handle_exit in the following file: linux/arch/x86/kvm/vmx/vmx.c

### Build the updated code: 

1. After changing the code in KVM as per the assignment requirement, you can rebuild using the same “make” sequence commands or simply use the below command.
```
sudo make -j 2 modules M=arch/x86/kvm 
```
2. Load and unload the kvm kernel module (kvm.ko) and kvm-intel module (kvm-intel.ko) using the following commands:
```
sudo rmmod arch/x86/kvm/kvm-intel.ko
```
```
sudo rmmod arch/x86/kvm/kvm.ko
```
```
sudo insmod arch/x86/kvm/kvm.ko
```
```
sudo insmod arch/x86/kvm/kvm-intel.ko
```
3. To test changes, we need to create a VM using virt manager.
Use the following command to install KVM, supporting packages and virt manager.
```
sudo apt-get update
```
```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager 
```
4. Verify KVM Installation using the below command. You should see an empty list of virtual machines. This indicates that everything is working correctly.
```
virsh -c qemu:///system list
```
5. Now open virtual machine manager and using Ubuntu ISO file create inner vm.

6. Open terminal and install cpuid using below command:
```
sudo apt-get update
```
```
sudo apt-get install cpuid
```
7. Create test program code in inner VM.

8. Once the Guest VM is up, issue the following shell script test_assignment2 few times:

   ./test_assignment2

After code changes:<br />

![After code changes](https://user-images.githubusercontent.com/78829969/142987823-3b828ec4-7bec-47d6-899b-9e577b9cf44c.PNG)

Virt Manager:<br />

![Virt Manager](https://user-images.githubusercontent.com/78829969/142987834-2cf8920a-57e7-4cda-a0eb-c46c6da81d85.PNG)

* Run the below commands in the inner VM (which is inside a VM):<br />  
  cpuid -l 0X4fffffff -s exit_number<br />
  cpuid -l 0X4ffffffe -s exit_number
  
  inner_vm:<br />
  
  ![image](https://user-images.githubusercontent.com/78829969/144047512-d7a6eafb-6bfc-48d7-b989-1cb5dd7ea5e7.png)

* Run the following command in the outer VM to get the output:<br />
  dmesg
  
  outer_vm:<br />
  
  ![image](https://user-images.githubusercontent.com/78829969/144047625-b7c352e2-b297-4a77-a2cb-b56c2d7a3bb4.png)
