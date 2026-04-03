告警名称: BlogInstanceDown
监控对象: 阿里云服务器上的 My_blog 博客服务
判断依据: 通过 Prometheus 抓取 blog target 的结果（up 指标），判断博客的指标接口是否可达
异常条件: 当 Prometheus 抓取失败时，告警进入 pending 状态；若持续 2 分钟仍未恢复，则进入 firing 状态
观察窗口目的: 为了过滤容器启动顺序差异、网络短暂波动等正常瞬时现象，避免因短暂抓取失败而产生误报
严重级别: critical
测试方法: 停止 My_blog 容器，保持 Prometheus 继续运行。观察 target 状态是否变为抓取失败，告警是否在规定时间内进入 firing，Alertmanager 是否收到告警并发送邮件。再启动服务观察是否自动恢复。
