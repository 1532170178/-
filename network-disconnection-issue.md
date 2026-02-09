# Cursor Agent 网络断开连接问题

## 问题描述

在运行 Cursor Agent 时出现网络断开连接错误。

## 错误信息

```
[unknown] Network disconnected
OTe: [unknown] Network disconnected
```

## 错误堆栈追踪

```
at lpf (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:9097:38348)
at apf (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:9097:37251)
at Cpf (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:9098:5088)
at Tva.run (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:9098:9098)
at async Vyt.runAgentLoop (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:34206:57104)
at async mgc.streamFromAgentBackend (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:34255:7835)
at async mgc.getAgentStreamResponse (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:34255:8576)
at async qTe.submitChatMaybeAbortCurrent (vscode-file://vscode-app/d:/cursor/resources/app/out/vs/workbench/workbench.desktop.main.js:9180:14965)
```

## 问题分析

错误发生在以下调用链中：
1. `submitChatMaybeAbortCurrent` - 提交聊天请求
2. `getAgentStreamResponse` - 获取 agent 流式响应
3. `streamFromAgentBackend` - 从 agent 后端流式传输
4. `runAgentLoop` - 运行 agent 循环
5. 在执行过程中发生网络断开连接

## 可能的原因

1. **网络不稳定**: 本地或远程网络连接中断
2. **超时问题**: 请求超时导致连接断开
3. **代理或防火墙**: 网络代理或防火墙阻止了连接
4. **服务端问题**: Cursor 后端服务暂时不可用
5. **请求过大**: 请求数据量过大导致连接中断

## 建议的解决方案

### 1. 检查网络连接
- 确认本地网络连接正常
- 检查是否可以访问其他在线服务
- 尝试 ping 测试网络连通性

### 2. 重试机制
- 实现指数退避重试策略
- 建议重试间隔：4秒、8秒、16秒、32秒
- 最多重试 4 次

### 3. 环境检查
- 检查代理设置
- 确认防火墙规则
- 验证 DNS 解析是否正常

### 4. 应用程序层面
- 重启 Cursor 编辑器
- 清除缓存
- 更新到最新版本

### 5. 日志收集
- 查看 Cursor 的详细日志
- 记录网络请求时间戳
- 监控网络状况

## 临时解决方案

如果问题持续出现：
1. 等待片刻后重试
2. 检查 Cursor 状态页面
3. 联系 Cursor 支持团队
4. 使用本地模式（如果可用）

## 记录时间

- Request ID: `3e1f65d6-8501-411b-9ef7-4c8908eb92ac`
- 日期: 2026-02-09

## 后续跟进

- [ ] 监控问题是否重复出现
- [ ] 收集更多日志信息
- [ ] 确定具体的网络失败点
- [ ] 实现重试逻辑（如适用）
