🚀 Edge Computing & Observability: From Cloud to ARM64 Mobile

    项目定位：基于一加手机 (ARM64) 边缘节点与阿里云公网网关的分布式监控与博客系统。
    Project Goal: Migrating cloud services to edge nodes using a Zero-Trust Network (Tailscale) and GitOps (GitHub Actions).

🏗 架构设计 (Architecture)

本项目打破了传统的“全站上云”模式，采用 边缘侧计算 + 云端网关 的混合架构：

    Cloud Gateway (阿里云): 部署 Nginx 作为公网入口，负责 SSL 卸载 (HTTPS) 和流量反向代理。
    Edge Node (OnePlus 6): 运行 postmarketOS (Linux)，作为核心计算节点，承载 Docker 容器化业务。
    Networking (Tailscale): 构建基于 WireGuard 的加密私有网络，实现跨地域设备直连与内网穿透。
    CI/CD (GitOps): 代码推送即部署，在 ARM 边缘端实现原地构建 (Build on Edge)。

Docker ContainersHTTPSTailscale 隧道GitHub ActionsSSHGit Pull & Rebuild访问者阿里云 Nginx 网关一加手机 边缘节点FastAPI BlogPrometheusGrafanaAlertmanagerGitHub
 
  
  
 
🛠 技术栈 (Tech Stack)

    Backend: Python 3.9 / FastAPI / Jinja2
    Runtime: Docker / Docker Compose (network_mode: host)
    Observability: Prometheus / Grafana / Node Exporter / Alertmanager
    OS/Kernel: postmarketOS (Linux 6.x) / Rocky Linux 9 (Container Base)
    Gateway: Nginx (Reverse Proxy) / Let's Encrypt (SSL)
    Network: Tailscale (SD-WAN) / nftables (Firewall)
    Automation: GitHub Actions (CI/CD)

🔥 核心特性 (Hardcore Features)
1. 异构计算与原地构建 (Native ARM64 Build)

针对 x86 (云端) 与 ARM64 (边缘端) 的架构差异，放弃了传统的镜像仓库推送模式，采用 GitOps 原地构建 方案。通过 GitHub Actions 远程驱动边缘节点拉取源码并构建原生 ARM64 镜像，规避了交叉编译带来的兼容性问题。
2. 网络安全与零信任 (Zero Trust Security)

    手机节点不直接暴露任何公网端口，通过 Tailscale 建立加密隧道。
    配置宿主机级防火墙 nftables，仅放行 tailscale0 接口流量，实现极高的入站安全性。
    云端 Nginx 强制开启 HSTS 与 SSL 加密。

3. 全链路可观测性 (Full-Stack Observability)

    业务埋点：FastAPI 接入 Prometheus 中间件，实时监控 QPS、响应延迟及业务 404/500 状态。
    硬件监控：Node Exporter 采集手机 CPU 温度、内存压力、磁盘 IO 及网络流量。
    告警闭环：配置 Alertmanager 接入 SMTP 邮件通知，实现“服务宕机 -> 阈值触发 -> 邮件告警”的自动化闭环。

4. 边缘侧性能优化 (Edge Optimization)

    使用 Docker network_mode: host 模式，减少虚拟网桥带来的 NAT 损耗，提升边缘侧高并发处理能力。
    开启 Nginx Gzip 压缩与静态资源缓存，有效缓解手机上传带宽受限带来的延迟感。

📈 监控预览 (Monitoring Preview)

(建议在这里贴 1-2 张你 Grafana 的截图，非常加分！)
🛠 如何部署 (Deployment)

     基础设施准备:
        手机刷入 postmarketOS 并安装 Docker。
        所有节点加入同一个 Tailscale 网络。
     配置网关:
        在阿里云 Nginx 配置中指向手机的 Tailscale IP。
     自动化部署:
        在 GitHub Secrets 中配置 SERVER_IP, SERVER_USER, SSH_PRIVATE_KEY。
        git push 触发 GitHub Actions 自动流水线。

📅 路线图 (Roadmap)

     
    核心业务从阿里云向一加手机迁移。
     
    实现基于 Tailscale 的全链路监控与告警。
     
    落地 GitHub Actions 自动化 CI/CD 流水线。
     
    自建国内 DERP 节点优化 Tailscale 转发延迟。
     
    引入 Ansible 实现多节点配置自动化管理。

👨‍💻 Author

LonelyHorse - 重庆地区大一学生 / DevOps 爱好者

    Email: 2703023812@qq.com
    Blog: https://blog.lonelyhorse.top
