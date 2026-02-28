[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/aQEb8Lmc)
# COMP4651 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Saturday

---

### Name: Pang Zi Yang
### Student Id: 20994568
### Email: zypang@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

## Answer:

### Measurement Tool: **Phoronix Test Suite**

### Tool Configuration:

**CPU Test - 7-Zip (pts/compress-7zip):**
- **Configuration**: Default settings (3 test runs, auto-detected CPU threads)
- **Why**: Default parameters ensure reproducibility and standard comparisons; 3 runs average out system noise
- **Results represent**: **MIPS (Million Instructions Per Second)** - Higher = better CPU performance
  - Example: 4500 MIPS means the CPU processes 4.5 billion compression instructions/second

**Memory Test - RAMspeed (pts/ramspeed):**
- **Configuration**: Selected Average operation with Integer data type (3 test runs)
- **Why**: The Average operation provides a representative measure of overall memory performance across different access patterns, while Integer operations are most common in real-world applications. Three runs average out system noise.
- **Results represent**: MB/s (Megabytes per second) - Higher = better memory bandwidth
  - Example: 10388 MB/s means memory transfers ~10.4 GB of data per second

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |Compression: 3478 MIPS, Decompression: 3046 MIPS, Average: 3262 MIPS  |       10333.49 MB/s             |
    | `t2.medium`  |Compression: 10290 MIPS, Decompression: 6002 MIPS, Average: 8146 MIPS|          19914.99 MB/s          |
    | `c5d.large` |Compression: 7640 MIPS, Decompression: 4877 MIPS, Average: 6258 MIPS|       13789.87 MB/s          |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |       3620         |     0.326     |
    | `m5.large` - `m5.large`   |       4970         |     0.164     |
    | `c5n.large` - `c5n.large` |         4940       |     0.240     |
    | `t3.medium` - `c5n.large` |             2320   |     0.662     |
    | `m5.large` - `c5n.large`  |        3090        |     0.536     |
    | `m5.large` - `t3.medium`  |            4530    |     0.235     |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |       32.9         |    64.4      |
    | N. Virginia - N. Virginia |       4430         |    0.233      |
    | Oregon - Oregon           |       4370         |    0.214      |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
