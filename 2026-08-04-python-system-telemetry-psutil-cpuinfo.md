# System Telemetry & Hardware Profiling: `psutil` and `cpuinfo`

**Date:** 2026-08-04

**Category:** Python / System Administration / DevOps

Today I learned how to bridge the gap between Python scripts and the underlying Operating System and hardware. By utilizing `psutil` and `py-cpuinfo`, Python can act as a fully functional system monitoring agent, similar to the backend services that power Task Manager on Windows or `htop` on Linux.

## `psutil` (Process and System Utilities)  
**What it is:** A cross-platform library used to retrieve real-time information on running processes and dynamic system utilization (CPU, memory, disks, network, and sensors).

**Industry Context:** 
In enterprise environments, you never want a server to silently run out of RAM and crash (Out of Memory/OOM kill). DevOps teams use `psutil` to write background daemon scripts that continuously monitor server health. If CPU usage spikes above 90% for 5 minutes, the `psutil` script automatically fires an alert to the engineering team's Slack channel.

### Core Implementation Example:
```python
import psutil

# 1. CPU Telemetry
# interval=1 forces the script to block for 1 second to calculate true usage over time
cpu_usage = psutil.cpu_percent(interval=1, percpu=True)
print(f"Core Usage: {cpu_usage}") 

# 2. Memory (RAM) Telemetry
mem = psutil.virtual_memory()
print(f"Total RAM: {mem.total / (1024 ** 3):.2f} GB")
print(f"Available RAM: {mem.available / (1024 ** 3):.2f} GB")
print(f"RAM Usage: {mem.percent}%")

# 3. Process Management (The Task Manager equivalent)
# Fetching the top 3 processes consuming the most memory
processes = sorted(psutil.process_iter(['pid', 'name', 'memory_percent']), 
                   key=lambda p: p.info['memory_percent'], 
                   reverse=True)

for proc in processes[:3]:
    print(f"PID: {proc.info['pid']} | Name: {proc.info['name']} | RAM: {proc.info['memory_percent']:.2f}%")
    
```

## `py-cpuinfo` (Deep Hardware Profiling) 
*What it is:* While `psutil` tells you how much CPU is currently being used, `cpuinfo` tells you exactly what the CPU physically is. It directly queries the OS and the CPU's internal registers to fetch exact hardware specifications.

*Industry Context:*
When deploying heavy Machine Learning models (like TensorFlow or PyTorch), the code often requires specific physical CPU instructions (like `AVX2` or `AVX512`) to run efficiently. Engineers use `cpuinfo` in their deployment scripts to check the hardware architecture before the application boots. If the required CPU flags are missing, the script gracefully aborts instead of crashing violently later.

### Core Implementation Example:
```Python
import cpuinfo

# Query the hardware (Returns a massive dictionary of specs)
info = cpuinfo.get_cpu_info()

# 1. Basic Hardware Identity
print(f"Processor Name: {info['brand_raw']}")
print(f"Architecture: {info['arch']}")
print(f"Physical Cores: {info['count']}")

# 2. Clock Speeds
print(f"Advertised Speed: {info['hz_advertised_friendly']}")
print(f"Actual Current Speed: {info['hz_actual_friendly']}")

# 3. Low-Level Instruction Flags (Hardware capabilities)
flags = info['flags']
if 'avx2' in flags:
    print("System supports AVX2 vector instructions. ML models optimized!")
else:
    print("Warning: AVX2 not found. Compute will be slower.")

```
## The Dream Team: Building a Node Exporter 
In modern Cloud Native architectures, these two libraries are often combined to create a Node Exporter (a telemetry agent).

- **Boot Phase (`cpuinfo`):** When the Python agent starts on a new EC2 server, it runs `cpuinfo` once to log the static hardware identity (e.g., "AMD EPYC, 16 Cores, x86_64") to the central database.
- **Runtime Phase (`psutil`):** The script then enters an infinite while loop, using `psutil` every 5 seconds to capture live dynamic metrics (Disk I/O, Network Packets Sent, RAM Usage) and pushes them to a visualization dashboard like Grafana.
