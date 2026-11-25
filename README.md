<h1 align="center">MiaoYan Bilingual · NetNewsWire Theme</h1>

<p align="center">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT"/></a>
  <a href="https://netnewswire.com/"><img src="https://img.shields.io/badge/NetNewsWire-6.0%2B-0066CC.svg" alt="NetNewsWire 6.0+"/></a>
  <a href="https://github.com/huasanai/MiaoYan-Bilingual-NetNewsWire-Theme/releases"><img src="https://img.shields.io/github/v/tag/huasanai/MiaoYan-Bilingual-NetNewsWire-Theme?label=version" alt="Version"/></a>
</p>

<p align="center">保留妙言（MiaoYan）极简阅读美学，内置 <b>可切换的云端与本地翻译模型</b> 的 NetNewsWire 中英双语段落对照主题。</p>

> 本项目基于 [@tw93](https://github.com/tw93) 开源的 [MiaoYan-NetNewsWire-Theme](https://github.com/tw93/MiaoYan-NetNewsWire-Theme) 二次开发，完整保留了原版妙言主题的字体、色彩与中文排版细节，在此基础上新增了纯前端流式双语段落对照翻译能力。非常感谢 Tw93 的开源设计！

---

## 核心特性

- **原汁原味的妙言排版美学**：延续仓耳今楷（`TsangerJinKai02`）字体、`0.5px` 字距、`1.7` 舒适行高与墨绿点缀色；中文译文以 `0.92em` 字号与柔和次要文本灰紧贴英文段落正下方，完整适配系统浅色与深色模式（Dark Mode）。
- **翻译栏可手动展开**：每篇文章都有 API 设置入口。默认在英文文章自动展开翻译栏，也可设置为始终展开或手动展开。
- **自定义翻译模型**：保留 DeepSeek 和 Gemini 预设，可添加多个 OpenAI Chat Completions 兼容 API。每个配置有独立的名称、地址、密钥和模型 ID，可指定新文章的默认模型。
- **3 路并发流式渲染 + 按模型隔离缓存**：限制最大并发请求数为 `3` 防止触发接口限流，未完成段落显示轻量呼吸灯提示并逐字流式输出；译文按「文章 + 模型配置 + 接口地址 + 模型名」独立存入本地缓存，来回切换模型对比或二次打开文章**秒出结果、不重复消耗 Token**。
- **阅读器深度适配**：自动保护代码块与行内 `code` 专业术语原貌；单段请求超时或失败支持段落级独立点击重试。

---

## 安装与快速开始

### 1. 安装主题

- **一键安装（推荐）**：复制下方地址到 Safari 等浏览器地址栏并回车，即可自动唤醒 NetNewsWire 完成安装：
  ```text
  netnewswire://theme/add?url=https://github.com/huasanai/MiaoYan-Bilingual-NetNewsWire-Theme/releases/latest/download/MiaoYan-Bilingual.nnwtheme.zip
  ```
- **手动安装**：前往 [Releases](https://github.com/huasanai/MiaoYan-Bilingual-NetNewsWire-Theme/releases) 下载最新的 `MiaoYan-Bilingual.nnwtheme.zip`，解压后双击 `MiaoYan-Bilingual.nnwtheme` 即可导入。
- 获取阅读器：[NetNewsWire 官网](https://netnewswire.com/)（支持 macOS / iOS / iPadOS）。

### 2. 配置翻译模型

1. 在 **NetNewsWire → Settings → General → Article Theme** 选择 **MiaoYan Bilingual**，打开任意文章。
2. 点击标题下方的 **API 设置**。在 **编辑模型** 中选择 DeepSeek 或 Gemini 填写密钥，也可以点 **添加模型**，填写显示名称、Base URL 及所需密钥。点击 **获取模型** 即可从服务的 `/models` 列表中选择，也可手动输入或粘贴模型名。点击 Model Name 旁的 **!** 可查看 Mac 单键快捷键提示。
3. 接入本机 Ollama 时，若服务监听默认端口，Base URL 填 `http://localhost:11434/v1`，Model Name 填本机 `ollama list` 中的完整模型名。免鉴权服务的 API Key 留空即可。主题会在 Base URL 后追加 `/chat/completions`。若从另一台设备阅读，请填写阅读设备可访问的服务地址。
4. 在 **新文章默认模型** 选择常用配置，点击 **保存设置**。旧版的模型参数与上次所选模型会自动沿用。
5. 点击 **翻译全文**。文章工具栏的下拉框只切换当前文章；打开下一篇会重新使用默认模型。已缓存的译文可直接切回查看。

> **隐私说明**：你的 API Key 与译文缓存仅保存在本地设备 NetNewsWire WebKit 容器的 `localStorage` 中，直接请求你所配置的模型接口，不经过任何第三方服务器。

---

## 目录结构

```text
MiaoYan-Bilingual-NetNewsWire-Theme/
├── Info.plist               # 主题元数据（MiaoYan Bilingual / 唯一标识符）
├── template.html            # 文章 HTML 模板 + 多模型翻译与缓存引擎
├── stylesheet.css           # 妙言基础排版样式 + 双语对照与深浅色主题样式
├── TsangerJinKai02-W04.ttf  # 仓耳今楷正文字体
├── LICENSE                  # MIT 开源协议
└── README.md                # 项目说明文档
```

---

## 设计原则

- **克制不抢戏**：辅助工具应当隐形于阅读流之中，不改变原版妙言的留白、字重与行距节奏。
- **零外部构建依赖**：单包内聚原生 HTML / CSS / JS，无需本地起代理服务或安装插件，双击即装即用。
- **Token 珍惜原则**：段落级增量落盘与多模型隔离缓存，中断续翻或模型横评都不花冤枉钱。

`v0.1.0` 是首个公开版本，后续更新可关注 [Releases](https://github.com/huasanai/MiaoYan-Bilingual-NetNewsWire-Theme/releases)。欢迎通过 [Issues](https://github.com/huasanai/MiaoYan-Bilingual-NetNewsWire-Theme/issues) 反馈问题或建议。

---

## 支持与交流

### 联系画伞

| 平台 | 账号 |
|---|---|
| 💬 微信 | `huasanai` |
| 🐦 X | [@yfusionai](https://x.com/yfusionai) |
| 🎬 抖音 | [@画伞](https://v.douyin.com/zHu4VUhztes/) |

---

## 致谢（Acknowledgments）

- 特别感谢 [@tw93](https://github.com/tw93) 设计并开源了 [MiaoYan-NetNewsWire-Theme](https://github.com/tw93/MiaoYan-NetNewsWire-Theme)，本项目的视觉基础与中文排版调校均源自妙言主题。
- 感谢 [NetNewsWire](https://netnewswire.com/) 团队持续维护如此纯粹、开放的 RSS 阅读器。

---

## License

基于 [MIT License](./LICENSE) 开源。
