# AstrBot 安装来源

本插件的 GitHub **仓库名** 与 `metadata.yaml` 的插件包 `name` 是两个不同字段。AstrBot 的插件身份由 `author/name` 决定，而“从链接安装”需要填写真实存在的 GitHub 仓库地址。

## 正确的仓库安装地址

```text
https://github.com/zjj1280637679-ship-it/huoshanfangzhougongyingshang
```

也可以在 AstrBot 的“从链接安装”输入：

```text
zjj1280637679-ship-it/huoshanfangzhougongyingshang
```

## 不要作为 GitHub 仓库地址使用

下面这个值是插件的包标识 `metadata.name`，不是当前 GitHub 仓库名：

```text
astrbot_plugin_volcengine_native_video_provider
```

因此不要把下面这个不存在的路径用于“从链接安装”：

```text
zjj1280637679-ship-it/astrbot_plugin_volcengine_native_video_provider
```

仓库根目录必须直接包含 `metadata.yaml`。本仓库的 GitHub Actions 会使用真实 AstrBot 安装器下载当前候选分支、确认安装根目录存在 `metadata.yaml`，并启动 AstrBot 验证插件和两个火山方舟 Provider 能成功加载后，才允许把候选视为可发布。
