Week 1 – System Planning and Distribution Selection

This week was mainly about getting everything set up properly before moving into the more
technical parts of the coursework. I installed Ubuntu Server inside VirtualBox, set up the
networking, and gathered the basic system information that I’ll need later on. The aim was to
build a stable and clean environment that I can use for the remaining weeks.


1. System Architecture Diagram

My setup for this coursework uses two systems:

- My Windows PC, which acts as the workstation where I will eventually manage the server.
- An Ubuntu Server VM running inside VirtualBox, which is the machine I will configure and
secure throughout the project.

Here is a simple diagram of how the systems are connected:

<img width="376" height="396" alt="image" src="https://github.com/user-attachments/assets/87e309ab-0df2-48db-85b2-6f444c83b475" />










The idea is that the server stays isolated but still reachable for remote access when I start using SSH.


2. Distribution Selection Justification

I decided to use 'Ubuntu Server LTS' for the server. I chose it because it’s stable, well-documented,
and widely used, which makes it easier to troubleshoot or look things up when needed.
The LTS version also gets long-term updates, which is useful since security is a big part of this coursework.

I considered other options like Debian or CentOS, but Ubuntu felt like the best balance between ease of use,
good package support, and reliability. It also installs as a minimal system by default, which matches the
requirement to manage the server through the command line only.


3. Workstation Configuration Decision

For the workstation, I decided to just use my 'Windows host machine' rather than setting up a second VM.
This keeps things simple and uses fewer resources. Windows also has built-in SSH support, so I can easily
connect to the Ubuntu Server once I configure SSH in a later week.  
This setup still follows the requirement of having a separate workstation connecting to the server remotely.



4. Network Configuration Documentation (VirtualBox Settings + IPs)

I configured the server VM with two network adapters:

Adapter 1 – NAT:
- Gives the server internet access for updates and package installation.
- Inside Ubuntu, this appears as `enp0s3`.
- The IP address it received was `10.0.2.15`.

Adapter 2 – Host-Only Adapter:
- Creates a private network between my Windows PC and the VM.
- This will be the connection I use later for SSH.
- Inside Ubuntu, this appears as `enp0s8`.
- The host-only IP address it received was:  
  192.168.56.101 (from `ip addr`).

Using NAT + Host-Only keeps the server isolated while still allowing controlled access for administration.



5. System Specifications (CLI Evidence)

As part of the setup, I ran the required system information commands and recorded the output.
Please find the screenshots of each command included below.

5.1 `uname'
<img width="205" height="32" alt="image" src="https://github.com/user-attachments/assets/1a975927-c025-456a-b54b-66e7fab4d5df" />


5.2 `free'
<img width="683" height="63" alt="image" src="https://github.com/user-attachments/assets/c4335aab-fdcc-470c-b837-9877eb61c9a7" />


5.3 `df'
<img width="725" height="130" alt="image" src="https://github.com/user-attachments/assets/5ec46d93-b352-447d-97e3-fbe8ddd4abe9" />


5.4 `ip addr'
<img width="861" height="336" alt="image" src="https://github.com/user-attachments/assets/03628b15-ac48-417a-ba57-53b866173308" />


5.5 `lsb_release'
<img width="253" height="37" alt="image" src="https://github.com/user-attachments/assets/c9a0ef65-34b2-4d24-af04-1af853e2b332" />




Reflection:

Overall, Week 1 was mostly about getting the environment ready. I installed Ubuntu Server,
set up the networking in VirtualBox, and collected the system details that I’ll need later on.
Now that everything is in place and working properly, I’m ready to move on to Week 2 and start
planning the security setup.

