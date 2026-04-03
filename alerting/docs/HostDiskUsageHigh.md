告警名称: HostDiskUsageHigh
监控对象: 阿里云 Linux 服务器的根目录 (/) 磁盘空间使用率
判断依据: 通过 node_exporter 采集的 `node_filesystem_size_bytes` 与 `node_filesystem_avail_bytes` 计算真实磁盘使用百分比
异常条件: 根目录磁盘使用率大于 80%
观察窗口目的: 过滤因临时生成大文件打包或系统短时写入大量日志，随后又被迅速清理而造成的瞬时可用空间波动，避免误报
严重级别: critical
测试方法:
1. 链路验证（安全）：临时修改告警规则阈值，将其调低至低于当前实际使用率（例如 > 10%），观察是否能成功触发告警邮件闭环。
2. 故障注入（实验）：在服务器使用 `fallocate -l 10G /tmp/dummy_file` 瞬间生成占位大文件触发阈值，测试成功后立即删除。
