# CMPE 283: Virtualization Technologies 
# Assignment 4: Nested Paging vs. Shadow Paging
This assignment builds upon assignment-3. The purpose of this assignment is to illustrate the difference in performance when using nested paging versus shadow paging and to illustrate the different exit frequencies and types. 

## Team Members: 
* Haard Sujal Shah (SJSU ID: 014702558)
* Ronak Kalpesh Prajapti (SJSU ID: 015271958)

## Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.

### Haard Sujal Shah (SJSU ID: 014702558)
- Collaborated with my teammate over the zoom call.
- Scripted the test file for the scenario posted in the assignment-4 instruction pdf.
- Ran the program with ept = 0, observed the results and captured the screenshots.
- Simulating the answers for the questions in the README.md file.

### Ronak Kalpesh Prajapati (SJSU ID: 015271958)
- Collaborated with my teammate over the zoom call.
- With ept != 0, executed the code, made observations and captured a screenshot.
- Verified results in both nested paging and shadow paging.
- Discussed with my teammate to conclude the observations made.

## Steps for completing the assignment:

**Steps Followed:**

* Run the assignment 3 code and boot a test VM using that code. 
* Once the VM has booted, record total exit count information (total count for each type of exit handled by KVM). 
* Shutdown the test (inner) VM.
* Remove the ‘kvm-intel’ module from your running kernel:
```
rmmod kvm-intel
```
* Reload the kvm-intel module with the parameter ept=0 (this will disable nested paging and force KVM to use shadow paging instead).
```
insmod /lib/modules/5/kernel/arch/x86/kvm/kvm-intel.ko ept=0 
```
* Boot the same test VM again, and capture the same output as you did in the second step. 
* On performing the above steps, we were successfully able to boot the VM by enabling Shadow paging.

## Q2. Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.
### Screenshots: 
#### Nested paging:
![alt text](/images/nested.png?raw=true)

#### Shadow paging:
Boot:
![alt text](/images/shadow1.png?raw=true)

After reboot:
![alt text](/images/shadow2.png?raw=true)

## Q3. What did you learn from the count of exits? Was the count what you expected? If not, why not?
In shadow paging, the number of exits increases when compared to nested paging. This is expected because during nested paging VM exit occurs when an EPT violation occurs. But in the case of shadow paging, it could exit every time the VM attempts to execute CR0, CR3, CR4 or any exits which are related to paging such as a page fault. 

## Q4. What changed between the two runs (ept vs no-ept)?
- **EPT Mode:** In this mode, we have two-layers of page tables which are used to translate from Guest VA to Guest PA and then to Host PA, and more page accesses are required, the guest VM should own page table and hence all the operations on CR3 is done in the same environment, i.e. no need to exit.

- **No EPT Mode:** Guest VM does not own the page table in shadow paging mode, for it, the VMM must simulate CR3.