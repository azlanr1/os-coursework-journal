Week 5: Advanced Security and Monitoring Infrastructure

This week focused on implementing advanced security controls and introducing monitoring and verification mechanisms on the Ubuntu Server. Building on the foundational security configuration from Week 4, the aim was to enforce mandatory access control, automate security updates, enhance intrusion detection, and create scripts that verify and monitor the system remotely. All tasks were carried out via SSH in line with the administrative constraints.

---

1. Mandatory Access Control Using AppArmor

To implement mandatory access control, AppArmor was used as it is enabled by default on Ubuntu and integrates directly with system services. AppArmor restricts what applications can access and provides an additional security layer beyond traditional user permissions.

The status of AppArmor was checked to confirm that it was enabled and actively enforcing security profiles. Reporting commands were then used to track which profiles were loaded and enforced on the system.

---

Output of sudo aa-status showing AppArmor enabled and profiles loaded:

(Multiple screenshots as the output did not fit into one image)

Screenshot 1:

<img width="992" height="932" alt="image" src="https://github.com/user-attachments/assets/2caa6256-851c-4f78-ba29-4ec54cece02a" />

Screenshot 2:

<img width="775" height="955" alt="image" src="https://github.com/user-attachments/assets/93915233-041e-4719-b837-063da7df0b5b" />

Screenshot 3:

<img width="848" height="962" alt="image" src="https://github.com/user-attachments/assets/7ac6651f-5b89-4201-983a-52f81b1eb1bd" />

Screenshot 4:

<img width="831" height="445" alt="image" src="https://github.com/user-attachments/assets/7f4f07d5-7c31-4bb0-83c4-e90baf7c72a4" />





To further demonstrate enforcement, the list of profiles running in enforce mode was displayed. This confirms that AppArmor is not only installed but actively applying restrictions.


Output of sudo aa-status --enforced showing enforced profiles:

<img width="550" height="89" alt="image" src="https://github.com/user-attachments/assets/b35c3066-0705-4c58-bf75-b1fa262f2567" />


Because SSH is a critical remote access service, its AppArmor profile was identified and verified to be running in enforce mode. This demonstrates that access control is applied to remote administration services.


AppArmor profile for SSH (/usr/sbin/sshd) shown in enforce mode:

Although AppArmor is enabled and enforcing profiles on the system, SSH does not have a dedicated AppArmor profile on this Ubuntu Server installation. This was confirmed by checking the loaded profiles in /etc/apparmor.d. Enforcement and reporting were instead demonstrated using aa-status, which confirms that mandatory access control is active and applied to system services. PLease see the screenshot attached below.

<img width="1085" height="761" alt="image" src="https://github.com/user-attachments/assets/63d46613-e6a3-40b7-ab0d-59e32140563d" />

---

2. Automatic Security Updates Configuration

To reduce the risk of unpatched vulnerabilities, automatic security updates were configured using the unattended-upgrades package. This ensures that important security patches are applied automatically without requiring manual intervention.

The unattended upgrades package was installed and enabled, and the configuration file was reviewed to confirm that security updates from Ubuntu repositories are applied automatically.


Installation of unattended-upgrades package:

<img width="877" height="200" alt="image" src="https://github.com/user-attachments/assets/ab3fa5e4-a6d1-42ed-a238-500933c0a9b5" />


The unattended upgrades configuration file was reviewed to verify that security updates are enabled.

Contents of /etc/apt/apt.conf.d/50unattended-upgrades showing security updates enabled:

<img width="1298" height="448" alt="image" src="https://github.com/user-attachments/assets/67a6ecf4-2ebb-46a6-b1da-9d3af7c6ce1f" />


This configuration ensures that the server remains protected against newly discovered vulnerabilities over time.

---

3. Intrusion Detection Using fail2ban

To protect the server against brute-force attacks, particularly targeting SSH, fail2ban was installed and configured. fail2ban monitors log files and automatically blocks IP addresses that show malicious behaviour, such as repeated failed login attempts.

The fail2ban service was installed and enabled on the server.

Installation of fail2ban package:

Screenshot 1:
<img width="1433" height="962" alt="image" src="https://github.com/user-attachments/assets/911a599a-6544-4c33-afd0-002343efd285" />

Screenshot 2:
<img width="1712" height="966" alt="image" src="https://github.com/user-attachments/assets/a8336a70-facb-4786-8493-9d87b8cea1c9" />



After installation, the SSH jail was enabled using the default configuration, and the status of fail2ban was checked to confirm that it was running and monitoring SSH login attempts.

Output of sudo fail2ban-client status showing service running:

<img width="1346" height="216" alt="image" src="https://github.com/user-attachments/assets/f99e8918-2a3e-4e77-a688-daf880b3f011" />


Output of sudo fail2ban-client status sshd showing SSH jail active:

<img width="825" height="269" alt="image" src="https://github.com/user-attachments/assets/39e47271-3bd7-44f8-a505-29aada18ff0a" />


---

4. Security Baseline Verification Script (Server-Side)

To verify that all security controls from Weeks 4 and 5 are correctly implemented, a security baseline verification script named security-baseline.sh was created on the server. This script checks key security configurations and reports their status.

The script verifies:

SSH configuration (key-based authentication and password login disabled)

Firewall status and SSH restriction

AppArmor enforcement status

fail2ban service and SSH jail status

Automatic security updates configuration

All commands in the script are commented line by line to explain their purpose.

Contents of security-baseline.sh showing commented security checks:

<img width="1637" height="870" alt="image" src="https://github.com/user-attachments/assets/a4ae1923-5803-4b8a-9346-1608327f63ac" />


The script was executed remotely via SSH to demonstrate that it runs successfully and reports the current security state of the server.

Execution of security-baseline.sh via SSH with output displayed:

Screenshot 1:

<img width="1001" height="960" alt="image" src="https://github.com/user-attachments/assets/f5ce6053-0521-4eee-9cc8-dd654356f594" />

Screenshot 2:

<img width="1151" height="967" alt="image" src="https://github.com/user-attachments/assets/b25c615f-9a38-4c68-90cb-72c32fd355a6" />

Screenshot 3:

<img width="941" height="958" alt="image" src="https://github.com/user-attachments/assets/4c432386-4e4a-4759-bdd7-bc1515cd3d72" />

Screenshot 4:

<img width="887" height="957" alt="image" src="https://github.com/user-attachments/assets/6b6ebb3e-5767-4079-9fda-d4460a92ede2" />

Screenshot 5:

<img width="1026" height="403" alt="image" src="https://github.com/user-attachments/assets/f03a7fc1-2243-4b69-8f0f-9b2143a74dd8" />



---

5. Remote Monitoring Script (Workstation-Side)

To introduce basic remote monitoring, a monitoring script named monitor-server.sh was created on the workstation. This script connects to the server via SSH and collects performance metrics such as CPU usage, memory usage, disk usage, and system uptime.

The script demonstrates how performance data can be collected remotely without direct access to the server console. Each command in the script is commented to explain what metric it collects and why it is useful.

Contents of monitor-server.sh showing SSH-based monitoring commands:

<img width="1417" height="697" alt="image" src="https://github.com/user-attachments/assets/f4ba34d1-7067-49b2-bb53-57148a3146de" />


The monitoring script was executed from the workstation, connecting to the server and displaying live system metrics.

Execution of monitor-server.sh showing collected server performance metrics:

<img width="880" height="585" alt="image" src="https://github.com/user-attachments/assets/96e9484c-fba0-4bf2-9caa-9b20d085accf" />


---

Reflection:

This week extended the server’s security beyond basic hardening by introducing mandatory access control, automated patching, and intrusion detection. Creating verification and monitoring scripts also demonstrated how security configurations can be audited and monitored remotely. These changes significantly improve the resilience of the system and reflect real-world server security practices used in production environments.
