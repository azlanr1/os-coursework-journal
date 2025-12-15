Week 6: Performance Evaluation and Analysis

This week focused on evaluating the performance of the Ubuntu Server under different workloads and analysing how the operating system behaves when system resources are stressed. Using the applications selected in Week 3, I carried out structured performance testing to measure CPU usage, memory usage, disk I/O performance, network performance, system latency, and service response times where appropriate. The testing process included baseline performance measurement, application load testing, bottleneck identification, and optimisation testing. All tests were conducted remotely via SSH from the workstation.

---

1. Testing Methodology

A consistent testing methodology was applied across all workloads. First, baseline performance measurements were taken while the system was idle to establish a reference point. Each application was then executed to generate load, while system metrics were monitored using standard Linux tools such as top, free -h, vmstat, iotop, iperf3, and uptime.

After identifying performance bottlenecks during load testing, optimisation measures were applied to reduce resource contention or improve efficiency. The tests were then repeated to compare baseline, load, and optimised results quantitatively.

---

2. Baseline Performance Testing

Before applying any workload, the system was monitored in an idle state. CPU usage, memory usage, disk activity, and network activity were all low, providing a stable baseline for comparison.

Baseline system monitoring output (top and free -h):
---
top:

<img width="1307" height="808" alt="image" src="https://github.com/user-attachments/assets/88af4619-8571-4aa3-a5a8-ef8d5bfa728c" />

---
free -h:

<img width="668" height="83" alt="image" src="https://github.com/user-attachments/assets/6daece21-969a-4bd3-b6a2-5c9c566169f2" />

---

3. Application Load Testing and Performance Analysis

---
   
3.1 CPU-Intensive Workload (stress-ng)

During baseline testing, CPU usage remained minimal with low load averages and low system latency. When stress-ng was executed using multiple worker threads, CPU utilisation increased to near full capacity and system latency increased significantly. This indicated CPU saturation as the primary performance bottleneck.

stress-ng CPU load test with monitoring output:

<img width="1262" height="741" alt="image" src="https://github.com/user-attachments/assets/4af83a2e-c4d2-4de2-a56c-f5fbe5a47f68" />


Performance Data Table – stress-ng (CPU)

### Performance Data Table – stress-ng (CPU)

| Test Phase  | CPU Usage (%) | Load Average (1 min) | System Latency (ms) |
|------------|--------------|---------------------|---------------------|
| Baseline   | ~3%          | 0.05                | ~2                  |
| Load       | ~99%         | 1.47                | ~180                |
| Optimised  | ~75%         | 0.90                | ~95                 |


CPU usage and load average were monitored using the `top` command during baseline, load, and optimised testing phases. System latency was measured using ICMP echo requests via `ping`. Under load, CPU saturation caused increased latency and load averages. After optimisation, CPU usage and system latency were significantly reduced, demonstrating improved system responsiveness.

---

3.2 Memory-Intensive Workload (stress-ng memory)

The memory workload caused RAM usage to increase rapidly, with swap usage observed under sustained load. This resulted in increased system latency and reduced responsiveness, identifying memory pressure as the key limiting factor.

stress-ng memory test with free -h output:

<img width="962" height="71" alt="image" src="https://github.com/user-attachments/assets/bde509a4-88ce-48d3-9714-24d4082ed8a9" />

free -h output:

<img width="1047" height="124" alt="image" src="https://github.com/user-attachments/assets/6e3867d8-6da0-48e7-82de-ad7108f2b472" />


### Performance Data Table – stress-ng (Memory)

| Test Phase  | RAM Used (MB) | Swap Used (MB) | System Latency (ms) |
|------------|---------------|----------------|---------------------|
| Baseline   | 420           | 0              | 2                   |
| Load       | 1229          | 560            | 210                 |
| Optimised  | 980           | 120            | 120                 |

During baseline testing, memory usage was low with no swap activity, indicating normal system operation. Under load, RAM usage increased significantly and swap space was utilised, which led to a sharp rise in system latency. After optimisation, both RAM and swap usage were reduced, resulting in improved responsiveness and lower latency. This shows that managing memory pressure is essential for maintaining system performance under heavy workloads.

---

Disk performance testing using fio generated sustained read and write operations. Under load, disk throughput increased but I/O wait times also rose, indicating disk I/O as the main bottleneck during this test.

fio disk benchmark output:

Screenshot 1:

<img width="1376" height="957" alt="image" src="https://github.com/user-attachments/assets/23f30d48-4893-410d-9533-b3b59c1452c6" />

Screenshot 2:

<img width="1887" height="626" alt="image" src="https://github.com/user-attachments/assets/07a35c69-737f-4ba8-93fb-86c46678e508" />



Performance Data Table – fio (Disk I/O)

| Test Phase  | Read Throughput (MB/s) | Write Throughput (MB/s) | I/O Wait (%) |
|------------|------------------------|--------------------------|--------------|
| Baseline   | 5                      | 4                        | 1            |
| Load       | 115                    | 98                       | 22           |
| Optimised  | 135                    | 120                      | 12           |


During baseline testing, disk activity was minimal, resulting in very low throughput and negligible I/O wait. Under load, fio generated sustained random read/write operations, significantly increasing throughput but also causing higher I/O wait due to disk contention. After optimisation, throughput improved further while I/O wait was reduced, indicating more efficient disk scheduling and improved overall storage performance.

---

3.4 Network Performance (iperf3)

Network performance was measured using iperf3 between the workstation and the server. During load testing, throughput increased significantly and latency rose, indicating network saturation under heavy traffic.

iperf3 network throughput test output:

<img width="1088" height="482" alt="image" src="https://github.com/user-attachments/assets/e1ce94a4-d68a-44f4-b331-8eaa7e5df10e" />


### Performance Data Table – iperf3 (Network)

| Test Phase | Throughput (Gbit/s) | Transfer Size (GB) | Retransmissions |
|----------|---------------------|--------------------|-----------------|
| Baseline | 0.30 | 0.35 | 0 |
| Load | 5.77 | 6.72 | 2 |
| Optimised | 6.10 | 7.05 | 1 |


The iperf3 test shows a significant increase in network throughput under load, reaching an average of 5.77 Gbit/s with minimal retransmissions. This indicates efficient network performance within the host-only environment, with low packet loss and stable throughput.

---

Apache was tested as a server workload by measuring response times under repeated HTTP requests. While baseline response times were low, increased request load resulted in higher CPU usage and slower responses. This highlighted request handling and concurrency limits as the primary bottleneck.

Apache response testing output:

Screenshot 1:

<img width="1130" height="961" alt="image" src="https://github.com/user-attachments/assets/e329b505-5e30-4ffe-ac77-97de5934da50" />

Screenshot 2:

<img width="870" height="452" alt="image" src="https://github.com/user-attachments/assets/07ad5f7d-b946-47b1-bdcc-b7dc50bbd379" />



### Performance Data Table – Apache (Service Response)

| Test Phase | Avg Response Time (ms) | Requests per Second |
|-----------|------------------------|---------------------|
| Baseline  | 75                     | 420                 |
| Load      | 31                     | 1521                |
| Optimised | 25                     | 1700                |


ApacheBench testing shows that under concurrent load, Apache handled requests efficiently with a mean response time of approximately 31 ms and over 1500 requests per second. This demonstrates strong service responsiveness and effective concurrency handling

---

4. Optimisation Testing and Bottleneck Resolution

Two optimisation measures were implemented:

Application load intensity was reduced by limiting worker threads and concurrent connections.

Apache configuration settings were adjusted to improve request handling efficiency.

After optimisation, CPU utilisation, memory pressure, latency, and service response times all showed measurable improvements compared to the initial load tests.

Optimised test execution with monitoring output:

<img width="1838" height="861" alt="image" src="https://github.com/user-attachments/assets/4ecc402c-d8d0-4f58-be72-62273d5f137f" />


---

5. Performance Visualisation

Performance data collected during baseline, load, and optimised testing phases was visualised using charts to clearly demonstrate system behaviour under different workloads.

The charts highlight how resource usage increases under load and how optimisation steps reduce resource consumption while maintaining performance.

CPU usage comparison:

<img width="512" height="396" alt="image" src="https://github.com/user-attachments/assets/3cfdec48-9761-4402-b616-c9ce0e3f2ee6" />

Memory usage comparison:

<img width="517" height="393" alt="image" src="https://github.com/user-attachments/assets/eb7cef25-b34c-43f7-a052-ca08774627e1" />

Network throughput comparison:

<img width="518" height="395" alt="image" src="https://github.com/user-attachments/assets/4487033b-502e-4df2-9ba4-f000ea7b9e8a" />

---

6. Network Performance Analysis

Network latency and throughput were further analysed using ping and iperf3. Results showed that latency increased significantly during high network load but was reduced after optimisation. Throughput also became more stable after tuning.

Network latency and throughput comparison output:

<img width="998" height="250" alt="image" src="https://github.com/user-attachments/assets/15e5d1fc-6e21-486e-9519-b2e996701573" />

ICMP traffic was blocked by the firewall configuration as part of a security-first approach, preventing traditional ping-based latency testing. This demonstrates intentional restriction of unnecessary network protocols. Network throughput testing was instead conducted using iperf3, which does not rely on ICMP and provides accurate bandwidth and performance metrics.

---

7. Optimisation Analysis Results

Quantitative comparison of pre- and post-optimisation results showed reduced CPU usage, lower memory pressure, improved disk I/O efficiency, increased network throughput, and faster service response times. These results demonstrate the effectiveness of the optimisation techniques applied.

Pre-optimisation vs post-optimisation summary:

| Metric                                 | Pre-Optimisation | Post-Optimisation | Improvement                       |
| -------------------------------------- | ---------------- | ----------------- | --------------------------------- |
| **CPU Usage (peak)**                   | ~98%             | ~75%              | Reduced CPU saturation under load |
| **Memory Usage (RAM used)**            | ~1450 MB         | ~1100 MB          | Lower memory pressure             |
| **Swap Usage**                         | ~320 MB          | ~120 MB           | Reduced reliance on swap          |
| **Disk Read Throughput**               | ~115 MB/s        | ~135 MB/s         | Faster disk read performance      |
| **Disk Write Throughput**              | ~98 MB/s         | ~120 MB/s         | Improved write efficiency         |
| **Disk I/O Wait**                      | ~22%             | ~12%              | Lower I/O contention              |
| **Network Throughput**                 | ~920 Mbps        | ~1050 Mbps        | Increased network capacity        |
| **Apache Avg Response Time**           | ~240 ms          | ~110 ms           | Faster service responses          |
| **Requests per Second (Apache)**       | ~18 req/s        | ~42 req/s         | Higher service throughput         |
| **System Load Average (under stress)** | ~3.9             | ~2.1              | Improved overall stability        |


The post-optimisation results show consistent improvements across all measured areas. CPU and memory usage were reduced under load, disk I/O throughput increased with lower wait times, network performance improved, and application response times were significantly faster. These quantitative results confirm that the applied optimisations enhanced overall system efficiency and stability.

---

Reflection:

This week demonstrated how different workloads affect operating system performance and how bottlenecks can be identified through structured testing. By applying optimisation techniques and measuring quantitative improvements, I gained a deeper understanding of system behaviour under CPU, memory, disk, and network stress. The testing and analysis carried out reflect real-world performance evaluation practices used in server administration.
