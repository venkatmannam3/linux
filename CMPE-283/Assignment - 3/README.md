# CMPE 283: Virtualization Technologies
# Assignment 3: Instrumentation via hypercall (cont’d)
The assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.
      
* For CPUID leaf node %eax=0x4FFFFFFD:<br />
  ◦ Return the number of exits for the exit number provided (on input) in %ecx<br /> 
      This value should be returned in %eax
      
* For CPUID leaf node %eax=0x4FFFFFFC:<br />
  ◦ Return the time spent processing the exit number provided (on input) in %ecx<br />
      Return the high 32 bits of the total time spent for that exit in %ebx<br />
      Return the low 32 bits of the total time spent for that exit in %ecx

For leaf nodes 0x4FFFFFFD and 0x4FFFFFFC, if %ecx (on input) contains a value not defined by the SDM, return 0 in all %eax, %ebx, %ecx registers and return 0xFFFFFFFF in %edx. For exit types not enabled in KVM, return 0s in all four registers.

## Team Members: 
* Kiran Bala Devineni (SJSU ID: 015218866)
* Venkat Mannam (SJSU ID: 015263326)

## Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.

### Kiran Bala Devineni (SJSU ID: 015218866)

* Contribution mainly includes writing code for returning the number of exits for the exit number provided (on input), the time spent processing the exit number provided (on input) and the total time spent for that exit. 
* Made necessary changes to the code in the following files: linux/arch/x86/kvm -> cpuid.c and linux/arch/x86/kvm/vmx -> vmx.c 
* Simulating the answers for the questions in the README.md file.

### Venkat Mannam (SJSU ID: 015263326)

* Compiled the modified code and debugged complilation errors.
* Researched how to perform testing on kernel code.
* Contribution also includes writing the test program and shell script.

## Q2. Describe in detail the steps you used to complete the assignment. 

**Prerequisite:** A working assignment 1 configuration.

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
```
sudo virt-manager
```
6. Open terminal and install cpuid using below command:
```
sudo apt-get update
```
```
sudo apt-get install cpuid
```
7. Run the test script using bash command.
```
bash test.sh
```
8. Now run dmesg in the outer vm to view the ouput from running the test script.
```
dmesg
```
## Q3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail? 

## Q4. Of the exit types defined in the SDM, which are the most frequent? Least?
