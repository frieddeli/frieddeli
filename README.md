<div align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&pause=1200&color=38BDF8&center=true&vCenter=true&random=false&width=650&height=50&lines=hey%2C%20i%27m%20ray%20%F0%9F%91%8B;welcome%20to%20my%20workshop;%27things%20are%20only%20impossible%20until%20they%27re%20not.%27%20%E2%80%94%20picard;breaking%20bare-metal%20locally%20so%20prod%20stays%20up;hunting%20for%20tesla%20v100s%20in%20the%20delta%20quadrant;%27make%20it%20so.%27%20%F0%9F%96%96" alt="Typing Banner" />
</div>

<br/>

<pre>
╭─── ray@homelab [~] ─────────────────────────────────────────────────────────────╮
│ $ cat about.me                                                                  │
│ I'm a Computer Engineering student at NTU Singapore, currently interning on     │
│ Google's Data Center team managing GCP production infrastructure.               │
│                                                                                 │
│ Previously at AMD. Off the clock, I run a self-hosted AMD EPYC + MI50 server in │
│ my room (hunting for used Tesla V100s to make this a proper cluster).           │
│ The best way to understand how systems break is to break them yourself.         │
│                                                                                 │
│ Founder & President of the NTU Semiconductor Club (400+ members).               │
│                                                                                 │
│ $ cat looking-for.txt                                                           │
│ Cloud Engineering, SRE, Physical AI, Platform & Inference Engineering (2027)    │
│                                                                                 │
│ $ fortune star-trek                                                             │
│ "Things are only impossible until they're not." — Captain Picard                │
╰─────────────────────────────────────────────────────────────────────────────────╯
</pre>

---

<details open>
  <summary><h3><code>$ pvesh get /nodes/ray/specs --verbose</code> // Bare-Metal Homelab & Cluster Evolution</h3></summary>

<br/>

> **Platform Overview:** Single-node enterprise hyperconverged infrastructure (HCI) running Proxmox VE (Debian Trixie base) on an AMD EPYC server with strict `cgroup v2` resource isolation, two-tier storage, and full-stack observability.

```text
┌──────────────────────────────────────────────────────────────────────────────────┐
│        PROXMOX HYPERCONVERGED ARCHITECTURE (AMD EPYC 7F52 · 16C/32T)             │
├──────────────────────────────┬─────────────────────────────┬─────────────────────┤
│ ACCELERATOR COMPUTE          │ STORAGE & NETWORK FABRIC    │ ISOLATION & KERNEL  │
│ • AMD Instinct MI50 32GB     │ • Tier-0: 2x NVMe (Hot)     │ • PREEMPT_DYNAMIC   │
│ • [Planned: +Tesla V100s]    │ • Tier-1: 2x HDD (Cold)     │ • cgroup v2 QoS     │
│ • Qwen 3.6 35B (custom ROCm) │ • 10G SFP+ Multi-VLAN       │ • ~40% Toil Cut     │
├──────────────────────────────┴─────────────────────────────┴─────────────────────┤
│ CO-RESIDENT TENANTS: InfluxDB/Grafana · Nginx PM · Nextcloud · Hermes AI · NTU VM│
└──────────────────────────────────────────────────────────────────────────────────┘
```

#### Hardware Architecture

| Component | Specification | Details / Role |
| :--- | :--- | :--- |
| <b>CPU</b> | AMD EPYC 7F52 | 16-Core / 32-Thread (up to 3.9 GHz boost, SP3 socket) |
| <b>GPU Accelerator</b> | AMD Radeon Instinct MI50 | 32 GB HBM2 (ROCm, power-capped to 150W via `amd-smi`) |
| <b>Chassis & Cooling</b> | Jonsbo N5 | Dense server chassis, high-static-pressure air cooling |
| <b>Storage (Hot Tier)</b> | 2× NVMe SSD (LVM) | High-IOPS root filesystems & latency-critical services |
| <b>Storage (Cold Tier)</b>| 2× Enterprise HDD (LVM)| Bulk archival storage, automated backups, and datasets |
| <b>Networking</b> | Multi-VLAN L2/L3 (10G SFP+) | Segregated management, DMZ, and internal tenant VLANs |
| <b>Kernel / Scheduling</b>| `PREEMPT_DYNAMIC` + `cgroup v2` | Hard CPU/RAM limits per tenant to eliminate noisy-neighbor interference |

#### Live Co-Resident Workloads & Tenants

| Service Class | Environment | Workload & Implementation Details |
| :--- | :--- | :--- |
| <b>GPU Inference</b> | Bare-metal LXC | **Qwen 3.6 35B** served via custom-compiled ROCm/llama.cpp build targeting Vega 20 (gfx906) |
| <b>Observability</b> | LXC / Docker | **Telegraf → InfluxDB → Grafana** monitoring CPU, GPU, memory, disk I/O, and network |
| <b>Reverse Proxy</b> | LXC / Docker | **Nginx Proxy Manager** with SSL termination and VLAN routing |
| <b>Object Storage</b> | LXC / VM | **Nextcloud** self-hosted sync and storage |
| <b>Cloud Dev Machine</b> | KVM VM | Isolated remote development environment |
| <b>Agent Workloads</b> | KVM VM | **Hermes** autonomous-agent VMs (GDG Singapore workshop infrastructure) |
| <b>Community Infra</b> | KVM VM | Hosting for **NTU Semiconductor Club** (400+ student organization) |

#### SRE & Reliability Highlights
* **Toil Reduction (~40%):** Automated backup scheduling, automated log rotation, and self-healing service health checks via Bash and cron.
* **SLO-Driven Observability:** Telegraf metrics streamed to InfluxDB with Grafana dashboards for proactive capacity planning and incident detection.
* **Network Fault Isolation:** Systematic packet-level diagnosis (`tcpdump`, `traceroute`, `netstat`) across segmented VLAN boundaries.

#### Cluster Roadmap & Hardware Evolution (In Progress)
* 🚀 **Multi-GPU Cluster Expansion:** Procuring enterprise accelerators (e.g., **NVIDIA Tesla V100s** / multi-GPU dense compute) to transition from single-accelerator testing to a proper multi-GPU cluster supporting distributed tensor parallelism and CUDA vs. ROCm comparative benchmarking.
* ☸️ **Declarative k3s Orchestration:** Redeploying a production k3s cluster provisioned entirely via **Terraform** and **Ansible** (reproducible IaC), migrating containerized inference workloads onto Kubernetes.
* 📈 **Inference-Native Autoscaling:** Implementing custom metrics-driven autoscaling for vLLM pods using Prometheus metrics (KV-cache saturation, queue depth `num_requests_waiting`, and P99 TTFT) rather than naive CPU/memory thresholds.
* 🤖 **Robot Simulation Farm:** Provisioning dedicated GPU-passthrough VMs running **NVIDIA Isaac Sim** and Gazebo to reproduce end-to-end cloud robotics digital twin pipelines (closing the loop from cloud infra to robot fleet).

<br/>
</details>

<details>
  <summary><h3><code>$ ./show_pipeline.sh --end-to-end</code> // Cloud-to-Robot Physical AI Through-Line</h3></summary>

<br/>

```text
┌───────────────────────────────────────────────────────────────────────────────┐
│                       THE CLOUD-TO-ROBOT PHYSICAL AI PIPELINE                 │
└───────────────────────────────────────┬───────────────────────────────────────┘
                                        ▼
┌──────────────────────────────┐  Kinesthetic   ┌───────────────────────────────┐
│ KINESTHETIC COLLECTION       │  Teleoperation │ DISTRIBUTED CLUSTER TRAINING  │
│ • 6-DoF Piper Robotic Arm    │ ─────────────► │ • Multi-Node (SLURM/k8s/Docker│
│ • CAN Bus Telemetry Stream   │                │ • VLASH-Forge Framework       │
│ • LeRobot Dataset Versioning │                │ • 8GB VRAM floor (QLoRA)      │
└──────────────────────────────┘                └───────────────┬───────────────┘
                                                                │ Optimized
                                TensorRT Compilation & FP16     │ Weights
                                29.5x Latency Reduction         ▼
┌───────────────────────────────────────────────────────────────────────────────┐
│ LOW-LATENCY EMBEDDED EDGE INFERENCE (Jetson AGX Orin)                         │
│ • π₀.₅ Vision-Language-Action (VLA) Model deployed on-robot                   │
│ • 184.5ms inference loop sustaining 30 Hz real-time closed-loop control       │
│ • Pick-and-place success lifted from 5% baseline to 65% on live hardware      │
└───────────────────────────────────────────────────────────────────────────────┘
```

<br/>
</details>

---

### Tech Stack & Operational Tooling

<table>
  <tr>
    <td width="50%" valign="top">
      <b>☁️ Cloud & Platform Infrastructure</b><br/>
      <img alt="GCP" src="https://img.shields.io/badge/-Google_Cloud-4285F4?style=flat-square&logo=google-cloud&logoColor=white" />
      <img alt="Proxmox" src="https://img.shields.io/badge/-Proxmox_VE-E57000?style=flat-square&logo=proxmox&logoColor=white" />
      <img alt="Kubernetes" src="https://img.shields.io/badge/-Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white" />
      <img alt="Docker" src="https://img.shields.io/badge/-Docker-2496ED?style=flat-square&logo=docker&logoColor=white" />
      <img alt="Terraform" src="https://img.shields.io/badge/-Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white" />
      <br/><br/>
      <b>⚡ Accelerators & Hardware Platforms</b><br/>
      <img alt="AMD EPYC" src="https://img.shields.io/badge/-AMD_EPYC-ED1C24?style=flat-square&logo=amd&logoColor=white" />
      <img alt="ROCm" src="https://img.shields.io/badge/-AMD_ROCm-ED1C24?style=flat-square&logo=amd&logoColor=white" />
      <img alt="CUDA" src="https://img.shields.io/badge/-NVIDIA_CUDA-76B900?style=flat-square&logo=nvidia&logoColor=white" />
      <img alt="Jetson" src="https://img.shields.io/badge/-Jetson_Orin-76B900?style=flat-square&logo=nvidia&logoColor=white" />
    </td>
    <td width="50%" valign="top">
      <b>📊 SRE, Observability & Core OS</b><br/>
      <img alt="Prometheus" src="https://img.shields.io/badge/-Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white" />
      <img alt="Grafana" src="https://img.shields.io/badge/-Grafana-F46800?style=flat-square&logo=grafana&logoColor=white" />
      <img alt="Linux" src="https://img.shields.io/badge/-Linux-FCC624?style=flat-square&logo=linux&logoColor=black" />
      <img alt="GitHub Actions" src="https://img.shields.io/badge/-GitHub_Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white" />
      <img alt="Git" src="https://img.shields.io/badge/-Git-F05032?style=flat-square&logo=git&logoColor=white" />
      <br/><br/>
      <b>🤖 Physical AI & Systems Engineering</b><br/>
      <img alt="PyTorch" src="https://img.shields.io/badge/-PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" />
      <img alt="ROS 2" src="https://img.shields.io/badge/-ROS_2-22314E?style=flat-square&logo=ros&logoColor=white" />
      <img alt="FastAPI" src="https://img.shields.io/badge/-FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" />
      <img alt="BigQuery" src="https://img.shields.io/badge/-BigQuery-4285F4?style=flat-square&logo=google-cloud&logoColor=white" />
      <img alt="C++" src="https://img.shields.io/badge/-C++-00599C?style=flat-square&logo=c%2B%2B&logoColor=white" />
      <img alt="Python" src="https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white" />
    </td>
  </tr>
</table>

---

### Featured Projects

<table>
  <thead align="center">
    <tr>
      <td><b>🎁 Project</b></td>
      <td><b>⭐ Stars</b></td>
      <td><b>📚 Forks</b></td>
      <td><b>🛎 Issues</b></td>
      <td><b>📬 Pull Requests</b></td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="https://github.com/Alvin0523/vlash-piper"><b>VLASH-Piper</b></a><br/><i>First known π₀.₅ deployment on Jetson AGX Orin; 29.5× latency reduction via async VLA inference, lifting pick-and-place success from 5% to 65%</i></td>
      <td><img alt="Stars" src="https://img.shields.io/github/stars/Alvin0523/vlash-piper?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Forks" src="https://img.shields.io/github/forks/Alvin0523/vlash-piper?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Issues" src="https://img.shields.io/github/issues/Alvin0523/vlash-piper?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Pull Requests" src="https://img.shields.io/github/issues-pr/Alvin0523/vlash-piper?style=flat-square&labelColor=343b41"/></td>
    </tr>
    <tr>
      <td><a href="https://github.com/Alvin0523/Depth-Anything-3-Underwater-Refinement"><b>DA3 Underwater</b></a><br/><i>LoRA fine-tuned Depth Anything 3 for AUV monocular depth; 59% AbsRel improvement, TensorRT-compiled for ROS 2 competition stack</i></td>
      <td><img alt="Stars" src="https://img.shields.io/github/stars/Alvin0523/Depth-Anything-3-Underwater-Refinement?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Forks" src="https://img.shields.io/github/forks/Alvin0523/Depth-Anything-3-Underwater-Refinement?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Issues" src="https://img.shields.io/github/issues/Alvin0523/Depth-Anything-3-Underwater-Refinement?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Pull Requests" src="https://img.shields.io/github/issues-pr/Alvin0523/Depth-Anything-3-Underwater-Refinement?style=flat-square&labelColor=343b41"/></td>
    </tr>
    <tr>
      <td><a href="https://github.com/frieddeli/VLASH-Forge"><b>VLASH-Forge</b></a><br/><i>Portable multi-node distributed training framework (SLURM, Docker, k8s) for robot VLA fine-tuning; reduced VRAM floor to 8 GB on consumer GPUs</i></td>
      <td><img alt="Stars" src="https://img.shields.io/github/stars/frieddeli/VLASH-Forge?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Forks" src="https://img.shields.io/github/forks/frieddeli/VLASH-Forge?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Issues" src="https://img.shields.io/github/issues/frieddeli/VLASH-Forge?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Pull Requests" src="https://img.shields.io/github/issues-pr/frieddeli/VLASH-Forge?style=flat-square&labelColor=343b41"/></td>
    </tr>
    <tr>
      <td><a href="https://github.com/frieddeli/NTU-GlobalProtect-for-Linux"><b>NTU GlobalProtect for Linux</b></a><br/><i>Fix for NTU's GlobalProtect VPN on Linux; saves NTU students hours of debugging</i></td>
      <td><img alt="Stars" src="https://img.shields.io/github/stars/frieddeli/NTU-GlobalProtect-for-Linux?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Forks" src="https://img.shields.io/github/forks/frieddeli/NTU-GlobalProtect-for-Linux?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Issues" src="https://img.shields.io/github/issues/frieddeli/NTU-GlobalProtect-for-Linux?style=flat-square&labelColor=343b41"/></td>
      <td><img alt="Pull Requests" src="https://img.shields.io/github/issues-pr/frieddeli/NTU-GlobalProtect-for-Linux?style=flat-square&labelColor=343b41"/></td>
    </tr>
  </tbody>
</table>

<br/>

---

### Activity Graph

<div align="center">
  <picture>
    <source srcset="dist/github-snake-dark.svg" media="(prefers-color-scheme: dark)">
    <source srcset="dist/github-snake.svg" media="(prefers-color-scheme: light)">
    <img alt="GitHub Snake" src="dist/github-snake.svg">
  </picture>
</div>

<br/>

---

### Connect

<p align="center">
  <a href="https://github.com/frieddeli" target="_blank"><img alt="GitHub" src="https://img.shields.io/badge/GitHub-%2312100E.svg?&style=for-the-badge&logo=github&logoColor=white" /></a>
  &nbsp;
  <a href="https://linkedin.com/in/ray-shao" target="_blank"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white" /></a>
  &nbsp;
  <a href="mailto:yshao004@e.ntu.edu.sg"><img alt="Email" src="https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white" /></a>
  &nbsp;
  <a href="https://frieddeli.github.io/Portfolio-Website/" target="_blank"><img alt="Portfolio" src="https://img.shields.io/badge/Portfolio-%23000000.svg?&style=for-the-badge&logo=firefox&logoColor=white" /></a>
</p>
