# CMPE 283: Virtualization Technologies 
# Assignment 4: Nested Paging vs. Shadow Paging
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

## Q2. Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.

## Q3. What did you learn from the count of exits? Was the count what you expected? If not, why not?

## Q4. What changed between the two runs (ept vs no-ept)?














    
    
