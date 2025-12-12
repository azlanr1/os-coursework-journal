Week 3: Application Selection for Performance Testing

This week focused on selecting a set of applications that represent different workload types so I can evaluate how my Ubuntu Server performs under different conditions. The aim was to choose tools that are simple to run, give clear performance differences, and allow me to monitor how the system behaves when stressed in various ways.

1. Application Selection Matrix

To cover a wide range of workloads, I selected five applications: one CPU-intensive, one RAM-intensive, one I/O-intensive, one network-intensive, and one server application. The following matrix shows each application and the justification for choosing it.

| Workload Type        | Application           | Reason for Selection |
|----------------------|-----------------------|-----------------------|
| CPU-Intensive        | stress-ng             | Generates heavy CPU load and is well documented; ideal for controlled CPU testing |
| RAM-Intensive        | stress-ng (memory)    | Allows specific memory allocation tests and shows how the system behaves under RAM pressure |
| I/O-Intensive        | fio                   | A flexible tool for disk read/write benchmarking and measuring storage performance |
| Network-Intensive    | iperf3                | Simple tool for generating network traffic and measuring upload/download throughput |
| Server Application   | Apache Web Server     | Provides a realistic server workload and allows me to test network access, concurrency and response behaviour |

2. Installation Documentation (SSH Commands)

All installations will be carried out remotely over SSH using the following commands.

2.1 CPU / RAM workload – stress-ng
```
sudo apt update
sudo apt install stress-ng
```



2.2. I/O workload – fio:

```
sudo apt update
sudo apt install fio
```

2.3. Network workload – iperf3
```
sudo apt update
sudo apt install iperf3
``` 



2.4. Server workload – Apache:
```
sudo apt update
sudo apt install apache2
```





3. Expected Resource Profiles

CPU-Intensive (stress-ng)
I expect stress-ng to use close to 100% of the CPU cores depending on the test parameters. The server may feel slower while the test is running.

RAM-Intensive (stress-ng memory mode)
I expect memory usage to rise quickly as large blocks of RAM are allocated. This may cause swapping and reduced performance if the VM runs out of available memory.

I/O-Intensive (fio)
fio will create heavy disk read/write activity. I expect higher disk utilisation, longer I/O wait times and slower responsiveness during testing.

Network-Intensive (iperf3)
iperf3 will generate strong inbound or outbound network traffic. I expect network usage to spike and possibly saturate the host-only interface depending on how long the test runs.

Server Workload (Apache)
Apache should use minimal resources when idle, but CPU and network activity will increase as more requests are made. This makes it useful for understanding how the server handles real traffic.

4. Monitoring Strategy

CPU-Intensive (stress-ng)
I will use top and htop to monitor CPU usage, load averages and general system responsiveness.

RAM-Intensive (stress-ng memory mode)
I will check memory usage with free -h and htop, and monitor swap usage to see how the server behaves when RAM becomes limited.

I/O-Intensive (fio)
I will use iotop to monitor disk activity and vmstat to observe I/O wait times.

Network-Intensive (iperf3)
I will use iftop or nload to monitor real-time network throughput and traffic patterns.

Server Workload (Apache)
I will use top for resource usage, iftop for network activity, and check the Apache logs in /var/log/apache2/ to understand how the server handles requests.

Reflection

This week helped me plan the practical performance testing I will carry out in later weeks. By selecting applications that represent different workload types, I now have a structured approach to analysing how my server behaves under CPU, memory, disk and network stress. Choosing Apache also gives me a realistic server workload to test. With installation commands, expected resource usage and monitoring methods all planned out, I am ready to begin the practical setup in Week 4.
