<h1>Hey, I'm Ray 👋</h1>

<p>
  I'm a <b>Computer Engineering student at NTU Singapore</b>, currently interning on <b>Google's Data Center team</b> managing GCP production infrastructure.<br/><br/>
  Previously at AMD building internal data platforms and developer tooling. I also run a <b>self-hosted AMD EPYC + MI50 hyperconverged infrastructure</b> on Proxmox VE at home, because the best way to understand how systems break is to break them yourself.<br/><br/>
  Looking for <b>Cloud Engineering, SRE, Physical AI, Platform and Inference Engineering</b> roles from 2027.
</p>

<pre><strong>$ cat ~/infrastructure/spec.yaml</strong>
apiVersion: platform.ray/v1
kind: SystemsEngineer
metadata:
  name: ray
  status: "Google Data Center Intern (GCP Infra) | NTU Singapore"
spec:
  hardware:
    host: "Single-node Hyperconverged Infrastructure (HCI) on Proxmox VE"
    cpu: "AMD EPYC 7F52 (16-Core / 32-Thread, SP3)"
    gpu: "AMD Radeon Instinct MI50 32GB HBM2 (ROCm)"
    storage: "Tiered LVM (2x NVMe hot tier + 2x HDD cold tier)"
    
  production:
    current: "Managing GCP Production Infrastructure @ Google Data Center"
    previous: "Internal Data Platforms & Developer Tooling @ AMD"
  seeking: "Cloud Engineering / SRE / Inference Platform roles (2027)"
</pre>

<details>
  <summary><b>🖥️ Homelab Hyperconverged Infrastructure & Hardware Specs (Click to expand)</b></summary>

<br/>

> **Platform Overview:** Single-node enterprise hyperconverged infrastructure (HCI) running Proxmox VE (Debian Trixie base) on an AMD EPYC server with strict `cgroup v2` resource isolation, two-tier storage, and full-stack observability.

#### Hardware Architecture

| Component | Specification | Details / Role |
| :--- | :--- | :--- |
| <b>CPU</b> | AMD EPYC 7F52 | 16-Core / 32-Thread (up to 3.9 GHz boost, SP3 socket) |
| <b>GPU Accelerator</b> | AMD Radeon Instinct MI50 | 32 GB HBM2 (ROCm, power-capped to 150W via `amd-smi`) |
| <b>Chassis & Cooling</b> | Jonsbo N5 | Server chassis, air cooling |
| <b>Storage (Hot Tier)</b> | 2× NVMe SSD (LVM) | High-IOPS root filesystems & latency-critical services |
| <b>Storage (Cold Tier)</b>| 2× Enterprise HDD (LVM)| Bulk archival storage, automated backups, and datasets |
| <b>Networking</b> | Multi-VLAN L2/L3 10G SFP+ | Segregated management, DMZ, and internal tenant VLANs |

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

<h3>Things I work with</h3>
<p>
  <img alt="GCP" src="https://img.shields.io/badge/-Google_Cloud-4285F4?style=flat-square&logo=google-cloud&logoColor=white" />
  <img alt="Proxmox" src="https://img.shields.io/badge/-Proxmox_VE-E57000?style=flat-square&logo=proxmox&logoColor=white" />
  <img alt="Kubernetes" src="https://img.shields.io/badge/-Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white" />
  <img alt="Docker" src="https://img.shields.io/badge/-Docker-2496ED?style=flat-square&logo=docker&logoColor=white" />
  <img alt="Terraform" src="https://img.shields.io/badge/-Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white" />
  <img alt="Prometheus" src="https://img.shields.io/badge/-Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white" />
  <img alt="Grafana" src="https://img.shields.io/badge/-Grafana-F46800?style=flat-square&logo=grafana&logoColor=white" />
  <img alt="GitHub Actions" src="https://img.shields.io/badge/-GitHub_Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white" />
  <img alt="Python" src="https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white" />
  <img alt="Linux" src="https://img.shields.io/badge/-Linux-FCC624?style=flat-square&logo=linux&logoColor=black" />
  <img alt="FastAPI" src="https://img.shields.io/badge/-FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" />
  <img alt="PyTorch" src="https://img.shields.io/badge/-PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" />
  <img alt="ROCm" src="https://img.shields.io/badge/-AMD_ROCm-ED1C24?style=flat-square&logo=amd&logoColor=white" />
  <img alt="ROS 2" src="https://img.shields.io/badge/-ROS_2-22314E?style=flat-square&logo=ros&logoColor=white" />
  <img alt="BigQuery" src="https://img.shields.io/badge/-BigQuery-4285F4?style=flat-square&logo=google-cloud&logoColor=white" />
  <img alt="React" src="https://img.shields.io/badge/-React-45b8d8?style=flat-square&logo=react&logoColor=white" />
  <img alt="C++" src="https://img.shields.io/badge/-C++-00599C?style=flat-square&logo=c%2B%2B&logoColor=white" />
  <img alt="Git" src="https://img.shields.io/badge/-Git-F05032?style=flat-square&logo=git&logoColor=white" />
</p>

<h3>Projects</h3>
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

<div align="center">
  <picture>
    <source srcset="dist/github-snake-dark.svg" media="(prefers-color-scheme: dark)">
    <source srcset="dist/github-snake.svg" media="(prefers-color-scheme: light)">
    <img alt="GitHub Snake" src="dist/github-snake.svg">
  </picture>
</div>

<br/>

<h3>Where to find me</h3>
<p>
  <a href="https://github.com/frieddeli" target="_blank"><img alt="GitHub" src="https://img.shields.io/badge/GitHub-%2312100E.svg?&style=for-the-badge&logo=github&logoColor=white" /></a>
  <a href="https://linkedin.com/in/ray-shao" target="_blank"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white" /></a>
  <a href="mailto:yshao004@e.ntu.edu.sg"><img alt="Email" src="https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white" /></a>
  <a href="https://frieddeli.github.io/Portfolio-Website/" target="_blank"><img alt="Portfolio" src="https://img.shields.io/badge/Portfolio-%23000000.svg?&style=for-the-badge&logo=firefox&logoColor=white" /></a>
</p>
