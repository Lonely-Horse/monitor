# Personal Observability & Alerting Stack 🚀

> 基于 Prometheus + Alertmanager + Grafana 构建的个人线上服务可观测性与自动化告警体系。
> 采用 **Infrastructure as Code (IaC)** 理念管理，并通过 GitHub Actions 实现配置的自动化热更新部署。

## 🌟 项目亮点

- **业务深度监控**：不仅监控主机存活，还针对 FastAPI 博客后端实现了细粒度的业务异常感知（如 `High404Rate`）。
- **告警路由闭环**：通过 Alertmanager 实现告警的分级路由、静默等待（`group_wait`）与防骚扰机制，最终通过安全加密的 SMTP 通道触达邮件。
- **GitOps 自动化交付**：全套监控配置托管于 GitHub，通过 Actions 自动同步至阿里云服务器并通过 `SIGHUP` 信号触发服务的平滑热重载，实现“配置即部署”。
- **安全配置分离**：通过模板渲染（`envsubst`）与 `.env` 文件严格分离敏感凭据（如 SMTP 授权码），确保开源仓库的绝对安全。

## 🏗️ 架构概览

```text
[ 业务服务 (FastAPI) ] --暴露 /metrics--> [ Prometheus ] <--拉取基础指标-- [ Node Exporter ]
                                            |
                                        触发规则评估
                                            v
                                     [ Alertmanager ] --按路由规则处理--> [ QQ 邮箱通知 ]
                                            |
                                      [ Grafana 大盘 ] (数据可视化)
