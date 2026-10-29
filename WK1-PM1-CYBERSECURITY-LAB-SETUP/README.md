Cybersecurity Lab Setup – Week 1

Project Overview

This project documents the setup and initial configuration of my cybersecurity lab environment using Kali Linux in a Virtual Machine (VM).

The objective of this task was to establish a functional virtual environment for cybersecurity practice and verify that the system was operational, with working network connectivity.

Objectives

* Set up and access a Kali Linux virtual machine.
* Become familiar with the Kali Linux graphical desktop environment.
* Use basic Linux terminal commands.
* Verify internet connectivity from the virtual machine.
* Confirm the currently logged-in user.
* Document the setup process and practical observations.

Tools and Technologies

* Kali Linux
* Virtual Machine (VM)
* Terminal
* ping
* whoami

Practical Implementation

Step 1: Accessing the Virtual Machine

I launched my Kali Linux virtual machine and accessed its graphical desktop environment.

This provided the working environment for carrying out the initial cybersecurity lab exercises.

Step 2: Testing Network Connectivity

I used the following command to test whether the virtual machine could communicate with an external host:

ping -c 3 google.com

The command sends three ICMP echo requests to the specified hostname.

The results showed that the virtual machine was able to receive responses, confirming that internet connectivity was available during the test.

Step 3: Identifying the Current User

I executed the following command:

whoami

The output was:

kali

This confirmed that the current terminal session was running under the kali user account.

Screenshots

Kali Linux Virtual Machine

The screenshot below shows the Kali Linux desktop environment with the terminal open.

It includes the network connectivity test and the user identification command.

Key Learnings

Through this exercise, I learned how to:

* Access and work within a Kali Linux virtual machine.
* Use basic Linux commands to verify system information.
* Test network connectivity using ICMP.
* Interpret basic ping output, including packet transmission and packet loss.
* Document cybersecurity lab activities for future reference.

Conclusion

This exercise provided a practical introduction to working within a virtual cybersecurity lab environment.

Successfully accessing the Kali Linux VM and verifying network connectivity established a foundation for further hands-on cybersecurity exercises.

I look forward to building on this experience as I continue developing my technical skills.

⸻

Program: Networkwalks Cybersecurity Internship
Task: Week 1 – Cybersecurity Lab Setup
Status: Completed
