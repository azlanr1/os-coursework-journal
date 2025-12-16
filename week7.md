Week 7: Security Audit and System Evaluation

This week focused on conducting a full security audit of the Ubuntu Server and evaluating the overall system configuration. Building on the security hardening implemented in previous weeks, the aim was to assess the current security posture, identify remaining weaknesses, and justify the system’s configuration using industry-standard security auditing tools.

The audit included host-based security scanning using Lynis, network security assessment using nmap, access control verification, a full service audit, and a review of system configuration settings. All tasks were performed remotely via SSH in line with the project constraints.

---

1. Security Audit Methodology

A structured audit methodology was followed to ensure comprehensive coverage of the system:

Host-based security auditing using Lynis to identify configuration weaknesses and security recommendations.

Network security assessment using nmap to identify exposed services and validate firewall rules.

Access control verification to confirm secure SSH configuration and user privilege management.

Service inventory audit to justify all running services and ensure no unnecessary services are active.

System configuration review to assess patching, firewall rules, intrusion detection, and mandatory access controls.

Risk assessment to identify residual risks that remain after remediation.

This approach mirrors real-world security auditing practices used in enterprise environments.

---

2. Host-Based Security Audit (Lynis)


Lynis was used to perform a comprehensive security scan of the system, covering kernel hardening, authentication mechanisms, file permissions, logging, and network configuration.

The initial scan provided a baseline security score and highlighted several warnings and suggestions related to system hardening.

Lynis audit execution command (sudo lynis audit system):

<img width="1232" height="942" alt="image" src="https://github.com/user-attachments/assets/d2a32bbd-119e-45df-af44-c697371041e2" />


Initial Lynis audit summary showing security score and warnings:

<img width="1416" height="881" alt="image" src="https://github.com/user-attachments/assets/2b6f1f55-de04-4e45-9230-383fe21afcff" />


2.1 Lynis Findings and Remediation

Based on the Lynis recommendations, the following remediation steps were verified or applied:

Automatic security updates confirmed via unattended-upgrades

Firewall configuration verified using UFW

SSH hardening confirmed (key-based authentication, root login disabled)

fail2ban confirmed active and monitoring SSH

AppArmor enforcement verified

After confirming these controls, Lynis was re-run to evaluate the improved security posture.

Post-remediation Lynis audit summary showing improved security score:

<img width="1492" height="866" alt="image" src="https://github.com/user-attachments/assets/1b765c25-d2aa-4cea-bcf1-344120a3299d" />


Lynis Security Score Comparison
Audit Phase	Lynis Security Score
Before remediation	60 / 100
After remediation	61 / 100

An initial Lynis security audit produced a hardening index of 60, highlighting several improvement areas related to service configuration and system hardening. Following targeted remediation actions — including firewall verification, mail service restriction, and service exposure reduction — the hardening index increased to 61.

The remaining recommendations primarily relate to advanced enterprise security controls (such as malware scanning, centralised logging, and disk encryption), which were considered out of scope for this project. These controls were not implemented deliberately, as the system operates within a controlled host-only environment and follows a security-first, minimal-exposure design.

The final Lynis score reflects a balanced approach between strong security controls and practical system requirements.

---

3. Network Security Assessment (nmap)


To evaluate the server’s network exposure, nmap was used to scan the server from the workstation. This allowed identification of open ports and verification that firewall rules were correctly enforced.

nmap scan command executed from the workstation:

<img width="1018" height="316" alt="image" src="https://github.com/user-attachments/assets/a09a990c-06e0-41bf-968b-411768046491" />


nmap scan output showing open ports:

The same screesnhot displayed earlier also shows the open ports.

<img width="1018" height="316" alt="image" src="https://github.com/user-attachments/assets/74d26512-97e4-416b-831b-b442374ccd26" />


3.1 Network Scan Analysis

The nmap scan revealed that:

Only port 22 (SSH) was open and accessible

No unnecessary services were exposed to the network

Firewall rules successfully restricted inbound traffic

This confirms that the server follows a principle of minimal exposure, significantly reducing the attack surface.

---

4. Access Control Verification

Access control mechanisms were reviewed to ensure that only authorised users can access the system and that privilege escalation is properly controlled.

The following controls were verified:

SSH key-based authentication enabled

Password authentication disabled

Root login over SSH disabled

Sudo access restricted to authorised users only

Relevant SSH configuration settings from /etc/ssh/sshd_config:

<img width="1203" height="873" alt="image" src="https://github.com/user-attachments/assets/921e2697-3895-4cc6-b1f2-4401048b8e09" />


Verification of user accounts and sudo privileges:

<img width="456" height="73" alt="image" src="https://github.com/user-attachments/assets/e2cb0015-70a5-4232-8c9e-3e4d0e13be5b" />

The sudo group membership was reviewed to verify administrative access control. Only authorised administrative users (azlan and adminuser) have sudo privileges, ensuring that elevated permissions are restricted and reducing the risk of unauthorised system changes.


These controls ensure strong authentication and prevent unauthorised administrative access.

---

5. Service Inventory and Justification

A full audit of running services was conducted to identify and justify each active service.

Output of systemctl list-units --type=service --state=running:

<img width="1418" height="921" alt="image" src="https://github.com/user-attachments/assets/f5e9ccbc-08a0-40ab-8766-4344847792fd" />

Service Inventory and Justification

ssh.service – Required for secure remote administration. Password authentication disabled and root login denied to reduce brute-force risk.

apache2.service – Required for web service testing and performance evaluation.

fail2ban.service – Provides intrusion prevention by blocking repeated failed authentication attempts.

cron.service – Required for scheduled system tasks and maintenance.

rsyslog.service – Centralised system logging for auditing and incident analysis.

unattended-upgrades.service – Automatically installs security updates to reduce exposure to known vulnerabilities.

systemd-networkd / resolved / timesyncd – Core networking and time synchronisation services required for system stability.

postfix@-.service
 – Local-only mail transfer used for system notifications; no external mail relay configured.

All running services were reviewed and justified. No unnecessary network-facing services were identified.

5.1 Running Services and Justification
Service Name	Purpose	Justification
sshd	Remote administration	Required for secure remote management
apache2	Web service testing	Required for performance testing
fail2ban	Intrusion detection	Protects against brute-force attacks
ufw	Firewall management	Enforces network access control
unattended-upgrades	Automatic patching	Ensures timely security updates
apparmor	Mandatory access control	Restricts application behaviour
systemd services	Core OS functions	Essential system processes

No unnecessary or unused services were identified, confirming a minimal and well-controlled service footprint.

---

6. System Configuration Review

The overall system configuration was reviewed to assess defence-in-depth controls:

Firewall: UFW enabled with restrictive inbound rules

Intrusion detection: fail2ban actively monitoring SSH

Mandatory access control: AppArmor enabled and enforcing

Patching: unattended-upgrades configured for security updates

Logging: system logs enabled and monitored

Firewall status (sudo ufw status verbose):

<img width="781" height="337" alt="image" src="https://github.com/user-attachments/assets/60aa6486-0086-4d49-8ff5-887d212eaca2" />


fail2ban status showing active SSH jail:

<img width="903" height="267" alt="image" src="https://github.com/user-attachments/assets/1dc6563e-9915-48f6-9fe4-36670725c685" />

Fail2Ban is active and configured to protect the SSH service. The sshd jail is enabled and monitoring authentication attempts via systemd logs. Although no IPs have been banned during testing, this confirms that automated intrusion prevention is in place to mitigate brute-force SSH attacks.




---

7. Remaining Risk Assessment


Despite the strong security posture, some residual risks remain:

Risk	Description	Mitigation Status
Zero-day vulnerabilities	Unknown exploits may exist	Mitigated via patching and monitoring
SSH exposure	SSH is externally accessible	Hardened with keys, fail2ban, firewall
Web service exposure	Apache running for testing	Limited scope and monitored

These risks are considered acceptable within the context of the system’s purpose and are mitigated using layered security controls.

---

8. Overall Security Evaluation

The security audit confirms that the Ubuntu Server is configured according to best practices for a hardened server environment. Security scanning, network assessment, access control verification, and service auditing all demonstrate a strong security posture with minimal attack surface.

The combination of proactive patching, mandatory access control, intrusion detection, and strict firewall rules provides effective defence-in-depth protection.

---

Reflection:

This final phase consolidated all security concepts applied throughout the project. Conducting a full security audit highlighted the importance of continuous assessment, rather than one-time hardening. Using tools such as Lynis and nmap provided practical insight into how real-world systems are evaluated and reinforced the need for layered security controls in production environments.
