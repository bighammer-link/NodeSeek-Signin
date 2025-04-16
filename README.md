# NodeSeek-Signin

## 📝 项目介绍

这是一个用于 NodeSeek 论坛自动签到的工具，支持通过 GitHub Actions 或青龙面板进行定时自动签到操作。签到模式默认为随机签到，帮助用户轻松获取论坛每日签到奖励。


## ✨ 功能特点

- 📅 支持 GitHub Actions 自动运行
- 🦉 支持青龙面板定时任务
- 🍪 支持 Cookie 或账号密码登录方式
- 🔐 支持多种验证码解决方案
  - 自建 CloudFreed 服务（免费）
  - YesCaptcha 商业服务（付费/赠送）
- 📱 支持多种通知推送渠道

## 🚀 使用方法

### 方式一：GitHub Actions

1. Fork 本仓库到自己的 GitHub 账号下
2. 在仓库的 `Settings > Secrets and variables > Actions` 中添加以下必要配置：

| 变量名称 | 必要性 | 说明 |
| :------: | :----: | :--- |
| `NS_COOKIE` | **建议** | NodeSeek 论坛的用户 Cookie，可在浏览器开发者工具(F12)的网络请求中获取 |
| `TG_BOT_TOKEN` | 可选 | Telegram 机器人的 Token，用于通知签到结果 |
| `TG_USER_ID` | 可选 | Telegram 用户 ID，用于接收通知 |
| `TG_THREAD_ID` | 可选 | Telegram 超级群组话题 ID，用于在特定话题中发送通知 |

> **注意**：若仅设置 Cookie 但未配置验证码服务，当 Cookie 过期后无法自动登录获取新 Cookie。

### 方式二：青龙面板

在青龙面板中执行以下命令克隆本仓库：

```bash
ql repo https://github.com/yowiv/NodeSeek-Signin.git
```

然后在环境变量中添加所需配置。

### 方式三：账号密码登录（自动获取新Cookie）

当 Cookie 失效时，系统会尝试使用账号密码方式登录并获取新的 Cookie。登录需要通过验证码验证，支持以下两种验证码解决方案：

#### 方案A：CloudFreed 自建服务（推荐家宽用户）

```bash
docker run -itd   --name cloudflyer   -p 3000:3000   --restart unless-stopped   jackzzs/cloudflyer -K 你的客户端密钥 -H 0.0.0.0
```

配置以下环境变量：

| 变量名称 | 说明 |
| :------: | :--- |
| `API_BASE_URL` | CloudFreed 服务地址，如 `http://192.168.1.100:3000` |
| `CLIENTT_KEY` | CloudFreed 服务的客户端密钥 |
| `USER` | NodeSeek 论坛用户名 |
| `PASS` | NodeSeek 论坛密码 |
| `SOLVER_TYPE` | 设置为 `turnstile`（默认值） |

#### 方案B：YesCaptcha 商业服务（推荐无法自建服务的用户）

1. 访问 [YesCaptcha](https://yescaptcha.com/i/k2Hy3Q) 注册账号
2. 注册后联系客服可免费获得余额（约可使用60次登录）
3. 配置以下环境变量：

| 变量名称 | 说明 |
| :------: | :--- |
| `CLIENTT_KEY` | YesCaptcha 的 API 密钥 |
| `USER` | NodeSeek 论坛用户名 |
| `PASS` | NodeSeek 论坛密码 |
| `SOLVER_TYPE` | 设置为 `yescaptcha` |

> **提示**：YesCaptcha 提供两个服务节点，可根据网络情况选择：
> - 国际节点：`https://api.yescaptcha.com`（默认）
> - 国内节点：`https://cn.yescaptcha.com`

## 🔧 环境变量完整说明

| 变量名称 | 必要性 | 默认值 | 说明 |
| :------: | :----: | :----: | :--- |
| `NS_COOKIE` | 建议 | - | NodeSeek 论坛的用户 Cookie |
| `USER` | 可选 | - | NodeSeek 论坛用户名，当 Cookie 失效时使用 |
| `PASS` | 可选 | - | NodeSeek 论坛密码 |
| `NS_RANDOM` | 可选 | true | 是否随机签到（true/false） |
| `SOLVER_TYPE` | 可选 | turnstile | 验证码解决方案（turnstile/yescaptcha） |
| `API_BASE_URL` | 条件必需 | - | CloudFreed 服务地址，当 SOLVER_TYPE=turnstile 时必填 |
| `CLIENTT_KEY` | 必需 | - | 验证码服务客户端密钥 |
| 各类通知变量 | 可选 | - | 支持多种推送通知平台配置 |

## 📊 验证码服务对比

| 服务 | 类型 | 优点 | 缺点 |
| :--: | :--: | :--- | :--- |
| CloudFreed | 自建服务 | 免费、无次数限制 | 需要自行部署维护 
| YesCaptcha | 商业服务 | 稳定可靠、易于配置 | 付费服务（有免费额度） 

## ⚠️ 免责声明

本项目仅供学习交流使用，请遵守 NodeSeek 论坛的相关规定和条款。
