# Advanced Computing Paradigms: Distributed, Parallel, & Cloud 

**Date:** 22-Jan-2026

Today I learned how computers work together to solve massive problems that a single machine can't handle. It is all about splitting the workload!

## 1. Distributed Computing 
**Definition:** A method where large problems are divided into smaller tasks and distributed across many computers.
* **Memory:** Each processor has its own private memory.

| Pros (Advantages) | Cons (Disadvantages) |
| :--- | :--- |
| **Speed:** Shared load increases processing speed. | **Complexity:** Requires extra programming effort. |
| **Reliability:** If one node fails, the system keeps working. | **Security:** Easier to track/hack in a network. |
| **Scalability:** Easy to add more nodes as needed. | **Network Reliance:** If the network fails, the system becomes unstable. |

## 2. Parallel Computing 
**Definition:** Many calculations are carried out simultaneously using shared memory.

### Serial vs. Parallel Processing

| Feature | Serial Computing | Parallel Computing |
| :--- | :--- | :--- |
| **Processor** | Single processor used. | Multiple processors with shared memory. |
| **Workflow** | Instructions executed sequentially (one after another). | Instructions executed simultaneously on different processors. |
| **Throughput** | Only one instruction at a time. | Multiple instructions at any moment. |

## 3. Grid Computing 
**Definition:** Using computational power like electricity from a grid. It allows unused resources on one computer to be used by another on the network.
* **Use Cases:** Disaster management, weather forecasting, bio-informatics.
* **Pros:** Solves massive problems quickly; maximizes hardware use.
* **Cons:** Slower interconnection speed; licensing issues across servers.

## 4. Cluster Computing 
**Definition:** Linking a group of personal computers and storage devices together to act like a single powerful computer.
* **Pros:** Great price/performance ratio; high availability.
* **Cons:** Hard to program; difficult to find faults (debugging is tough).

## 5. Cloud Computing 
**Definition:** Using internet-connected remote servers to store data and run applications instead of your local drive.
* **Service:** Delivered to the end-user as a service over a network.
* **Example:** E-mail services (Gmail!).

---
Whether it is a **Cluster** (computers grouped together locally) or a **Grid** (geographically separated resources), the goal is the same: Divide and Conquer!
