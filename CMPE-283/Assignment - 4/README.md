# CMPE 283: Virtualization Technologies 
# Assignment 4: Nested Paging vs. Shadow Paging
This assignment builds upon assignment-3. The purpose of this assignment is to illustrate the difference in performance when using nested paging versus shadow paging and to illustrate the different exit frequencies and types. 

NOTE – there is no coding required for this assignment. We are just running assignment-3 again in a different configuration. 

## Team Members: 
* Kiran Bala Devineni (SJSU ID: 015218866)
* Venkat Mannam (SJSU ID: 015263326)

## Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.

### Kiran Bala Devineni (SJSU ID: 015218866)
- Collaborated with my teammate over the zoom call.
- Scripted the test file for the scenario posted in the assignment-4 instruction pdf.
- Ran the program with ept = 0, observed the results and captured the screenshots.
- Simulating the answers for the questions in the README.md file.

### Venkat Mannam (SJSU ID: 015263326)
- Collaborated with my teammate over the zoom call.
- With ept != 0, executed the code, made observations and captured a screenshot.
- Verified results in both nested paging and shadow paging.
- Discussed with my teammate to conclude the observations made.

## Steps for completing the assignment:
**Prerequisite:** A working assignment-3.<br />

**Environment Setup:** We build this homework assignment on assignment-3 and hence used the same environment setup.

**Steps Followed:**

* Boot the Guest VM.
* Reboot the guest VM and, Record total exit count information.
* Shutdown the test (inner) VM.
* Remove the ‘kvm-intel’ module from your running kernel.
<img width="436" alt="image" src="https://user-images.githubusercontent.com/78829969/145257732-30eee885-03eb-4922-b44d-a7a118558395.png">

* Reload the kvm-intel module with the parameter ept=0 (this will disable nested paging and force KVM to use shadow paging instead).
<img width="472" alt="image" src="https://user-images.githubusercontent.com/78829969/145257881-9897d79a-595e-495a-baac-7b1c3d8a04a0.png">

* Boot the VM again, note the exits.
* Reboot the VM and record the exits.

## Q2. Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.

## Q3. What did you learn from the count of exits? Was the count what you expected? If not, why not?

## Q4. What changed between the two runs (ept vs no-ept)?














    
    
