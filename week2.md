Week 2: Security Planning and Threat Modelling

This week focused on planning how I am going to secure the Ubuntu Server VM before carrying
out any actual security configuration in later weeks. The aim was to understand what assets
need protecting, what threats exist, and what controls I will implement to reduce the risks.

Methodology:

To build the threat model for my server, I followed a simple step-by-step approach. I first
looked at what assets I actually need to protect in my setup, such as the VM itself, my
credentials, and the network connection. Once I understood what needed protecting, I
identified the main threats that could realistically affect a system like mine, for example
unauthorised access, brute-force attacks and misconfigurations. After listing the threats, I
reviewed my server in its current state to see what weaknesses it has before any security is
applied. Finally, I planned the security controls that I will implement in later weeks, making
sure each control directly addresses one of the threats I identified.  

This approach keeps the threat model focused on my actual system and makes it easier to
justify the security decisions I make later in the project.



1. Asset Identification

These are the main assets in my system that need protecting:

- Ubuntu Server VM – the core system that the coursework revolves around.
- User credentials – my login details and any future SSH keys.
- Configuration files – anything related to SSH, firewall rules, and system settings.
- System logs – important for monitoring suspicious activity.
- Network access – especially the Host-Only network connection used for administration.

Keeping these assets secure is essential because any compromise would affect the accuracy and
validity of the project.



2. Threat Identification

Based on my system setup, here are the main threats the server could face:

- Brute-force attacks on SSH – attackers repeatedly guessing passwords.
- Unauthorized remote access – especially if SSH is left open with weak credentials.
- Misconfiguration – incorrect firewall or SSH settings creating security holes.
- Malware or malicious packages – especially if the system is not kept updated.
- Network snooping – although limited due to the Host-Only network, NAT exposure still exists.

These threats represent realistic issues faced by Linux servers, even in isolated or virtualised
environments.



3. Vulnerability Identification

Right now, before doing any security hardening, the system has several weaknesses:

- SSH is installed with default settings and could be vulnerable if left unchanged.
- Password authentication is still enabled.
- No firewall rules are in place yet.
- The system relies on default ports (e.g., port 22 for SSH).
- No intrusion detection or rate limiting tools (like fail2ban) are active yet.
- System updates have not been configured or automated.

These vulnerabilities will be addressed in the coming weeks.



4. Planned Security Controls

Here are the controls I plan to implement to reduce the risks identified above:

- Create SSH key authentication and disable password login.
- Configure UFW firewall to allow only specific traffic (mainly SSH).
- Enable fail2ban to protect against brute-force login attempts.
- Regularly install system updates to patch security vulnerabilities.
- Review system logs to identify any suspicious behaviour.
- Restrict SSH settings (e.g., disabling root login, limiting users).




5. Linking Controls to Threats

| Threat                       | Planned Control                                    | Effect                                                 |
| ---------------------------- | -------------------------------------------------- | ------------------------------------------------------ |
| Brute force attacks on SSH   | fail2ban, SSH key authentication                   | Blocks repeated attempts and uses stronger credentials |
| Unauthorized access          | Firewall rules, disabling password login           | Limits access to trusted users only                    |
| Misconfiguration             | Reviewing SSH settings and applying best practices | Reduces accidental exposure                            |
| Malware or outdated packages | Regular system updates                             | Keeps the system patched and secure                    |
| Network-based attacks        | UFW firewall + Host-Only network                   | Reduces exposure to external networks                  |




Reflection:

This week helped me understand what risks my system could face and how to address them. I
now have a clear plan for securing the server, which I will begin implementing in the next
weeks. Having a solid threat model also makes it easier to justify the security choices I make
later in the project.

