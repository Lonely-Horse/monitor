告警名称: HostHighMemoryUsage
监控对象: 阿里云 Linux 服务器的真实可用内存 (Available Memory) 占比
判断依据: 重点通过 node_exporter 采集的 `node_memory_MemAvailable_bytes` (而非简单的 Free 内存) 来评估系统还能腾出多少内存给程序使用
异常条件: 真实内存使用率超过 85%
观察窗口目的: 防止因瞬间的重负载操作（如拉取巨大 Docker 镜像、临时数据备份脚本运行）导致的可用内存陡降引发误报，给予系统缓冲释放的时间
严重级别: warning
测试方法: 
1. 链路验证（安全）：临时将监控规则中的阈值修改为低于当前实际使用率，验证 Prometheus 到 Alertmanager 的路由机制。
2. 故障注入（实验）：使用专业压测工具 `stress-ng` 临时吃掉大量内存并设置超时自动释放，验证真实高负载下的告警表现。
