# Red-Vs-Blue-Project-2: Attack, Defense & Analysis of a Vulnerable Network
This project utilized Remote Desktop Protocol to access a pre-built VM which contained more VMs within its Hyper-V Manager.

![Network Diagram](Network_Diagram.PNG)

This document will cover a quick overview of the project by covering the goals of red team and blue team, as well as the steps they took to accomplish those goals. Specific details can be found in the [Reports directory](Reports) and in the [Project Presentation](/Project-Presentation/Project_Presentation.pdf).


## Blue Team Part 1

First, as Blue Team we configured watcher alerts in Kibana ahead of time so that we can see how they act as the attack is happening. These alerts could also be viewed in the Discovery page by creating a related index pattern for the Discovery page to pull from so that we could view the network traffic associated with them. This part relates to the [Blue Team Summary of Operations](/Reports/Blue_Team_Summary_of_Operations.docx) document found in the [Reports directory](Reports).

## Red Team

The goal of the Red Team was to gain root access to Target 1 using the Kali VM and to "capture flags" along the way. To do this, the username and password of the account with sudo privileges granted to python needed to be discovered, and then the escalation to root could occur by exploiting a python vulnerability which required having the aforementioned sudo privileges for python. This part relates to the [Red Team_ Summary of Operations](/Reports/Red_Team_Summary_of_Operations.docx) document found in the [Reports directory](Reports).

This was done over 8 steps.

Step 1: Scan the network to identify the IP address, and exposed ports and services of Target 1.

Step 2: Enumerate the WordPress site using "wpscan" in order to find usernames.

Step 3: Guess the password of the user "michael" in order to SSH into Target 1.

Step 4: Find the MySQL database username and password in the wp-config.php file.

Step 5: Use the credentials to log into MySQL and dump WordPress user password hashes.

Step 6: Crack the password hashes with John the Ripper coupled with rockyou.txt.

Step 7: Secure a user shell as "steven" using the password that was found. Steven is the account with sudo access to python.

Step 8: Escalate to root by exploiting the python vulnerability.

## Blue Team Part 2

After conducting the attack, we needed to find evidence of the attacks in Kibana. We used what we found to tailor new watcher alerts based on signatures that we deemed important. We also came up with ways to harden the system against it having weak passwords, privilege escalation via Python, a MySQL data breach, and the vulnerabilities presented by WordPress. Details can be found in the [Project Presentation](/Project-Presentation/Project_Presentation.pdf).

## Wireshark Activity

In this part, we were tasked to act as if the following scenario was playing out

"You are working as a Security Engineer for X-CORP, supporting the SOC infrastructure. The SOC analysts have noticed some discrepancies with alerting in the Kibana system and the manager has asked the Security Engineering team to investigate.

Yesterday, your team confirmed that newly created alerts are working. Today, you will monitor live traffic on the wire to detect any abnormalities that aren't reflected in the alerting system.

You are to report back all your findings to both the SOC manager and the Engineering Manager with appropriate analysis."

This part relates to the [Network Analysis](/Reports/Network_Analysis.docx) document found in the [Reports directory](Reports).

We needed to run a systemctl command to start a sniff on our Kali VM which would start a tcpreplay over Kali's eth0 interface which we could then capture using Wireshark. We then used the captured data to solve the 3 following issues: "Time thieves" who were spotted watching YouTube during work hours, at least one Windows host infected with a virus, and illegal downloads which were occurring. These 3 issues are divided into the 3 sections which are covered in the report.

