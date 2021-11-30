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

**Prerequisite:** A working previous assignment configuration and setup.

### Steps followed to implement the Assignment Functionalities:

1. Edit the files cpuid.c and vmx.c for implementing code for returning the number of exits for the exit number provided (on input), the time spent processing the exit number provided (on input) and the total time spent for that exit.<br />

      The modified files cpuid.c and vmx.c can be found at linux/arch/x86/kvm/cupid.c and linux/arch/x86/kvm/vmx/vmx.c respectively.

2. After changing the code in KVM as per the assignment requirement, compile the code using the following command: 
```
make -j 4 modules
```
And then run the following command:
```
sudo bash
```
3. Install the module packages: 
```
sudo make INSTALL_MOD_STRIP=1 modules_install && make install
```
4. Stop the nested VM which is currently running.

5. On the host VM, run the following commands:  
```
rmmod kvm_intel
```
```
rmmod kvm 
```
The above command removes the old binaries of the kvm.

6. Install the latest binaries using the below command,  
```
modprobe kvm
```
```
modprobe kvm_intel
```
7. Create an Inner Virtual Machine (inside a VM) using the below commands:
```
  sudo apt update
```
```
  sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
```
```
  sudo systemctl status libvirtd
```
```
  sudo systemctl enable --now libvirtd
```
```
  sudo apt install virt-manager
```
```
  sudo virt-manager
```
8. Install ubuntu 20.4 iso image. Also, download the CPUID debian Package and install it.

9. Run the below commands in the inner VM (which is inside a VM):<br />
  cpuid -l 0X4ffffffc -s exit_number<br />
  cpuid -l 0X4ffffffd -s exit_number

10. Run the following command in the outer VM to get the output:
```
dmesg
```

## Q3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail? 

## Q4. Of the exit types defined in the SDM, which are the most frequent? Least?

* According to the output:

The most frequent were, 

- #### Exit number 48 - EPT Violation
- #### Exit Number 7 - Interrupt window

The least frequent were,

- #### Exit number 29 - MOV DR
- #### Exit number 54 - WBINVD
- #### Exit number 55 - XSETBV
