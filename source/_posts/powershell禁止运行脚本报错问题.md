---
title: powershell禁止运行脚本报错问题
date: 2020-10-19 18:13:20
categories:
  - Tip
tags: tip
---

    powershell禁止运行脚本报错问题，windows环境原因导致

<!-- more -->
## 报错信息
![](/images/powershell-error.png)

## 解决方案
- win+X启动 windows PowerShell（管理员）
- 若要在本地计算机上运行您编写的未签名脚本和来自其他用户的签名脚本，请使用以下命令将计算机上的 执行策略更改为 RemoteSigned 执行：set-ExecutionPolicy RemoteSigned
- 查看执行策略：get-ExecutionPolicy

![](/images/powershell.png)


---