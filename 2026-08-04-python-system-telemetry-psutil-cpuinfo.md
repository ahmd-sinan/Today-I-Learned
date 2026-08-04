# System Telemetry & Hardware Profiling: `psutil` and `cpuinfo`

**Date:** 2026-08-04

**Category:** Python / System Administration / DevOps

Today I learned how to bridge the gap between Python scripts and the underlying Operating System and hardware. By utilizing `psutil` and `py-cpuinfo`, Python can act as a fully functional system monitoring agent, similar to the backend services that power Task Manager on Windows or `htop` on Linux.

## 1. `psutil` (Process and System Utilities)  
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