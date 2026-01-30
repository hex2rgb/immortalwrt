# Tailscale 配置指南

## 📋 Tailscale 简介

Tailscale 是一个基于 WireGuard 的零配置 VPN 解决方案，提供安全的点对点网络连接。

## 🔧 已启用配置

### 软件包配置 (`.config`)
```bash
CONFIG_PACKAGE_tailscale=y
```

### 主要特性
- ✅ 零配置网络
- ✅ 基于 WireGuard 的高性能加密
- ✅ NAT 穿透能力
- ✅ 自动 IP 地址分配
- ✅ 内置访问控制

## 🚀 使用场景

### 1. 家庭网络扩展
- 将远程设备接入家庭网络
- 安全访问家庭 NAS、打印机等设备
- 无需复杂 VPN 配置

### 2. 远程办公
- 安全连接公司内网资源
- 替代传统 VPN 解决方案
- 支持移动设备

### 3. IoT 设备管理
- 安全远程管理智能设备
- 访问内网摄像头、传感器
- 简化物联网部署

## ⚙️ 配置步骤

### 1. 初始设置

#### 在路由器上启动 Tailscale：
```bash
# 启动服务
/etc/init.d/tailscale start

# 启用开机自启
/etc/init.d/tailscale enable

# 查看服务状态
/etc/init.d/tailscale status
```

#### 认证设备：
```bash
# 生成认证链接
tailscale up

# 或者使用预授权密钥（推荐）
tailscale up --auth-key=tskey-xxxxxxxxxxxxxxxxxx
```

### 2. 网络配置

#### 查看分配的 IP：
```bash
# 查看 Tailscale IP
tailscale ip -4
tailscale ip -6

# 查看连接状态
tailscale status
```

#### 配置子网路由（可选）：
```bash
# 将本地子网通过 Tailscale 暴露
tailscale up --advertise-routes=192.168.123.0/24

# 接受子网路由（在其他设备上）
tailscale up --accept-routes
```

### 3. 访问控制

#### 使用 ACL 控制访问：
```bash
# 编辑 ACL 配置文件
vi /etc/tailscale/acl.json

# 示例 ACL 配置：
{
  "acls": [
    {
      "action": "accept",
      "src": ["autogroup:admin"],
      "dst": ["autogroup:admin:*"]
    },
    {
      "action": "accept",
      "src": ["*"],
      "dst": ["*:22"]
    }
  ]
}

# 应用 ACL 配置
tailscale set --acl-file=/etc/tailscale/acl.json
```

## 🔐 安全配置

### 1. 认证方式
- **推荐**：使用预授权密钥（Pre-auth keys）
- **位置**：Tailscale Admin Console → Keys
- **权限**：可限制设备权限和过期时间

### 2. 网络隔离
- 使用 ACL 限制设备间访问
- 启用 MagicDNS 简化设备发现
- 配置防火墙规则

### 3. 密钥管理
- 定期轮换预授权密钥
- 监控设备连接状态
- 及时移除不需要的设备

## 🔧 故障排除

### 常见问题

#### 1. 连接失败
```bash
# 检查服务状态
/etc/init.d/tailscale status

# 查看日志
logread | grep tailscale

# 重启服务
/etc/init.d/tailscale restart
```

#### 2. DNS 解析问题
```bash
# 启用 MagicDNS
tailscale set --accept-dns=true

# 检查 DNS 配置
tailscale dns
```

#### 3. 路由问题
```bash
# 检查路由表
tailscale netcheck

# 查看路由状态
tailscale ping <目标IP>
```

### 高级调试
```bash
# 详细日志
tailscale debug daemon-logs

# 网络诊断
tailscale debug netcheck

# 重置配置
tailscale down
tailscale up
```

## 📊 性能优化

### 网络配置
- 选择最近的 DERP 服务器
- 启用 UDP hole punching
- 优化 MTU 设置

### 系统资源
- 监控 CPU 和内存使用
- 调整日志级别
- 定期清理日志文件

## 🔄 集成方案

### 与 OpenClash 集成
Tailscale 可以与 OpenClash 配合使用：
- Tailscale 处理设备间连接
- OpenClash 处理代理流量
- 实现更灵活的网络架构

### 与 OpenVPN 集成
双重 VPN 保护：
- OpenVPN 提供基础 VPN
- Tailscale 提供设备级访问
- 分层安全防护

## 📚 相关文档

- [Tailscale 官方文档](https://tailscale.com/kb/)
- [Tailscale ACL 指南](https://tailscale.com/kb/1018/acls/)
- [WireGuard 技术说明](https://www.wireguard.com/)

---

**配置时间**：2026-01-29
**适用版本**：ImmortalWrt (基于 OpenWrt)
**功能**：零配置 VPN 解决方案</contents>
</xai:function_call">Write">
<parameter name="path">/home/builder/immortalwrt/Tailscale_Configuration_Guide.md