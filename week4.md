Week 4: Initial System Configuration and Security Implementation

This week focused on deploying the Ubuntu Server and implementing the first set of foundational security controls. All configuration tasks were carried out remotely via SSH from my Windows workstation, in line with the administrative constraint for this phase. The main objectives were to secure SSH access using key-based authentication, configure a restrictive firewall, manage users and privileges, and demonstrate secure remote administration.

---

1. SSH Key-Based Authentication Configuration

The first step was to secure SSH access by switching from password-based authentication to key-based authentication.

On my Windows workstation, I generated an SSH key pair using PowerShell. The public key was then copied to the server and added to the authorized_keys file for my user account. File permissions were adjusted to ensure the key could not be accessed by other users.

Once the key was in place, I edited the SSH configuration file to disable password authentication and prevent root login.

SSH configuration file before changes (/etc/ssh/sshd_config):
```
#PasswordAuthentication yes
#PermitRootLogin yes
```
SSH configuration file after changes:
```
PasswordAuthentication no
PermitRootLogin no
PubkeyAuthentication yes
```
After making these changes, the SSH service was restarted and key-based login was tested successfully from my workstation.
------
Key Inside authorized_keys:

<img width="1205" height="67" alt="image" src="https://github.com/user-attachments/assets/25e8acaa-5230-453b-a3ad-7c7b9cc0ed77" />

---
Successful SSH login using key-based authentication (no password prompt):

<img width="1021" height="693" alt="image" src="https://github.com/user-attachments/assets/a8db35d1-5640-422b-b4ca-c3e27d724149" />


---

/etc/ssh/sshd_config after changes:

<img width="686" height="326" alt="image" src="https://github.com/user-attachments/assets/c608b177-5448-49c1-889a-6fa93b8fb470" />


---

2. Firewall Configuration (Restricting SSH to One Workstation)

To further reduce the attack surface, I configured the UFW firewall so that SSH connections are only permitted from my specific workstation IP address on the Host-Only network.

Using SSH, I applied a firewall rule allowing port 22 access exclusively from my workstation and enabled the firewall. All other inbound connections are denied by default.

The firewall ruleset was verified to confirm that SSH access is restricted correctly.

---
Output of sudo ufw status numbered showing SSH allowed only from the workstation:

<img width="970" height="256" alt="image" src="https://github.com/user-attachments/assets/d2e9a381-b6dc-4b48-a458-8e5cb3bb8872" />

---

3. User Management and Privilege Control

To avoid relying on the default user for administration, I created a dedicated non-root administrative user. This user was added to the sudo group to allow controlled privilege escalation when required.

After creating the user, I logged in remotely via SSH using the new account and verified that administrative commands could be executed using sudo.

Creation of the non-root administrative user:

<img width="962" height="325" alt="image" src="https://github.com/user-attachments/assets/7f701e8e-b8b8-4799-9451-758e33562bca" />

---
Extra Evidence: 

<img width="931" height="160" alt="image" src="https://github.com/user-attachments/assets/cadaa19b-a412-45e4-93f8-33e8f65fd22c" />


---


Successful SSH login as the admin user:

<img width="1205" height="961" alt="image" src="https://github.com/user-attachments/assets/c8b15afe-69cb-4c31-977f-166057568407" />

---

The rest of the output as it was unable to fit into one screenshot:
<img width="952" height="292" alt="image" src="https://github.com/user-attachments/assets/33eb9ce4-7968-4e7d-83f9-3d4734a8c1e8" />

---


Execution of a sudo command (e.g. sudo apt update) over SSH:

<img width="1335" height="857" alt="image" src="https://github.com/user-attachments/assets/a8f39a32-ccd3-44d1-b345-e922cef805a6" />

---

4. SSH Access Evidence

SSH access was tested and confirmed at multiple stages:

Successful key-based login for the main user

Successful SSH login for the non-root administrative user

Password authentication disabled and no longer accepted

These tests confirm that SSH access is secured and functioning as intended.

SSH login confirmation showing key-based access:

<img width="1108" height="930" alt="image" src="https://github.com/user-attachments/assets/fdb674ee-b13b-41d3-97e7-5837a235d7ec" />

---


5. Firewall Ruleset Documentation

The firewall configuration was reviewed after all changes were applied to ensure rules were correct and active. The final ruleset confirms that:

SSH is allowed only from the workstation IP

All other inbound traffic is blocked by default

---

Final firewall ruleset using sudo ufw status verbose:

<img width="991" height="360" alt="image" src="https://github.com/user-attachments/assets/8bbfa2d4-6d3d-4007-bd8d-51329b1c4162" />

---

6. Remote Administration Evidence

All configuration tasks were performed remotely via SSH, including:

Editing system configuration files

Restarting services

Managing users and privileges

Enabling and verifying firewall rules

This demonstrates compliance with the administrative constraint that all server management must be carried out remotely.

Remote administration commands executed via SSH (e.g. restarting SSH or checking firewall status):

<img width="841" height="377" alt="image" src="https://github.com/user-attachments/assets/6ec41845-9a00-4247-aca5-1da35182fc7c" />

---

Reflection

This week marked the transition from planning to active system security implementation. By configuring key-based SSH authentication, restricting firewall access, and creating a dedicated administrative user, I significantly reduced the server’s attack surface. Completing all tasks remotely via SSH also reflects real-world server administration practices. The system is now securely configured and ready for more advanced security controls and monitoring in the following weeks.
