# RE-01 · Operit 逆向角色卡

配合 [`manyuegong33/r0crawl_skills`](https://github.com/manyuegong33/r0crawl_skills) 使用的 Operit 角色卡。
文件：`re-card.json`（Tavern / character_card_v2 格式）。

## 这张卡解决什么

Operit 的模型默认不知道「手机算力能干什么、不能干什么」，也不知道要先用技能包做路由。
这张卡把三件事固定下来：

1. **先判后动**：判定目标类型（APK / 网页接口 / 原生 so / 流量）+ 授权状态，再给路线
2. **证据优先**：结论必须带文件路径、行号、请求片段；不足就说不足
3. **算力自觉**：大反编译、JSVMP/WASM、批量并发 → 主动说明手机跑不动，建议移交服务器

## 安装

### 1. 装技能包 r0crawl_skills

Operit → 包管理 / 技能 → 从 GitHub 导入，填：

```
https://github.com/manyuegong33/r0crawl_skills
```

这个仓库的 `SKILL.md` 在**根目录**，可以直接识别，不需要指定子目录。
装完在技能列表里能看到 `r0crawl_skills`。

### 2. 装运行时（Operit 的 Ubuntu 终端里）

```bash
apt update && apt install -y python3 python3-pip nodejs npm curl jq unzip
pip3 install --break-system-packages httpx mitmproxy
```

自检，看这台机器能用哪些模块：

```bash
cd ~/skills/r0crawl_skills   # 按实际导入路径调整
python3 scripts/check_toolchain.py
```

### 3. 导入角色卡

下载 `re-card.json`，在 Operit 的角色卡页面导入（支持 Tavern JSON）。
导入后给这张卡绑定：

- **模型**：选你能用的最强模型，逆向任务对推理能力敏感
- **技能**：勾选 `r0crawl_skills`
- **工作区**：建一个专门放案件的目录，例如 `~/re-cases`

## 用法

开场直接给目标和材料，例如：

```
这是我自己的 App 客户端，已授权。APK 在 /sdcard/Download/app.apk，
我想搞清楚登录请求里的 sign 是怎么算的，最后要一个 Python 版本。
```

角色会先反问授权与交付形式，然后按「建 case → 静态拆 → 必要时动态 → 交付纯协议」的路线走。

## 已知限制（写在卡里，避免它吹牛）

| 能做 | 做不到 |
|---|---|
| 静态拆 JS / DEX / Manifest / 签名 | Chrome DevTools 断点、调用栈追踪 |
| 还原签名算法、写 Python collector | IDA / Ghidra 依赖的模块 |
| Frida 动态观察（需 root 或改包） | 大规模并发爬取、JSVMP/WASM 深挖 |
| Operit 内置浏览器 + hook 注入侦察 | 抓有 SSL pinning 的 App 流量（无 root） |

测试机参考：vivo X30（Exynos 980，2×A77 + 6×A55，8GB RAM，UFS 2.1，Android 10）。

## 边界

仅用于**你自己拥有或已获书面授权**的目标。绕过付费、绕过风控、攻击他人系统的请求，这张卡被写成直接拒绝。
