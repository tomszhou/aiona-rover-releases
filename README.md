# aiona-rover

跑在 macOS 上的 IM 网关：在微信 / 企业微信 / 钉钉里给机器人发任务，它驱动本机的
[Codex](https://developers.openai.com/codex/cli) 干活，把结果发回聊天窗口。Codex 要执行命令、
改文件或越出沙箱时先请求授权，你回一句才继续。

**这个仓库只用来发安装包。** 到 [Releases](../../releases) 取最新版。

## 安装

需要 **macOS（Apple Silicon）** 和 [Codex CLI](https://developers.openai.com/codex/cli)。

```bash
# 1. 校验（对一下 Release 页上写的那串）
shasum -a 256 aiona-rover-0.4.1-arm64.tar.gz

# 2. 解包
mkdir -p ~/aiona-rover
tar -xzf aiona-rover-0.4.1-arm64.tar.gz -C ~/aiona-rover

# 3. 装
cd ~/aiona-rover && ./scripts/setup.sh <你的名字>
```

```
0.4.1  SHA-256  6a9a7d990772c24270e79b1feafa849a551fab581b873e1dc1e76bf4c7b5e979
```

向导会带你走完：装 Codex CLI、登录、选平台、建工作目录、扫码授权、装成开机自启的服务，
最后把机器人的白名单从「谁都能用」收紧到「只有你」。

**用 `scp` 或内网 HTTP 传这个包。** 经浏览器或聊天软件传会被 macOS Gatekeeper 打上隔离标记
（向导会清，但不如一开始就别沾）。

## 一台机器装给几个人

带名字装就行，各自互不干扰——各有各的配置、服务、工作目录和 Codex 登录：

```bash
./scripts/setup.sh alice
./scripts/setup.sh bob
```

> 企业微信要注意：**一个机器人同一时刻只允许一条长连接**，所以每个实例必须用不同的 Bot ID。

## 升级

**不用停服务，也不用重跑 `setup.sh`**：

```bash
tar -xzf aiona-rover-<新版本>-arm64.tar.gz -C ~/aiona-rover
cd ~/aiona-rover && ./scripts/install-launch-agent.sh <你的名字>
```

一台机器上有几个实例的话，一个一个重启，每个之间确认一下。包内 `docs/OPERATIONS.md` 有完整的
升级和回滚说明。

## 卸载

```bash
cd ~/aiona-rover && ./scripts/uninstall.sh <你的名字>
```

默认只移除服务，配置、会话记录和你的工作目录都留着。要连数据一起删加 `--purge`——它仍然会
单独问你工作目录留不留。

## 文档

安装包里带着：`docs/MANUAL.md`（聊天里能用的指令）、`docs/WECHAT.md`、`docs/WECOM.md`、
`docs/DINGTALK.md`（各平台怎么配）、`docs/OPERATIONS.md`（运行、升级、排障）、
`docs/ISOLATION.md`（几个人共用一台机器时，隔离做得到什么、做不到什么）。

## 有问题

开个 [Issue](../../issues)。
