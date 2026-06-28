---
name: read-screenshot
description: 读取 iCopy 最新截图，识图并执行后续操作
trigger: 用户消息包含"读取截图"时加载此 skill
---

# 📸 read-screenshot — iCopy 截图读取

## 触发条件

用户消息中出现 **"读取截图"** 时，执行本流程

---

## Step 1：找到 iCopy 最新截图

```bash
ls -lt ~/Library/Containers/cn.better365.iCopy/Data/Documents/tempFiles/ | head -5
```

取最新（排第一）的 `.png` 文件路径，例如：
```
~/Library/Containers/cn.better365.iCopy/Data/Documents/tempFiles/iCopy_2026_06_28_1782635283208_8.png
```

> ⚠️ 如果目录不存在或无 `.png` 文件，告知用户"未找到 iCopy 截图"

---

## Step 2：读取图片内容

用 `Read` 工具读取该图片（绝对路径），Claude 原生视觉会识别图中文字和公式

---

## Step 3：执行用户后续指令

识图完成后，根据用户附带的要求执行操作，常见场景：

### 场景 A：写成错题

1. 加载模版 `Tools/模板/错题卡片模板.md`
2. 按用户指定的 `学科` / `章` / `重要程度` / `考察知识点` 填充 YAML
3. `> [!question]` 填入题干
4. 解答内容直接以正文写入（无 callout 包裹）
5. 保存到指定路径

### 场景 B：识图 + 上传 OSS

1. 用 `oss-upload --md <图片路径>` 上传到阿里云 OSS
2. 返回 `![](url)` 格式的外链
3. 本地不保存图片（遵守核心铁律）

### 场景 C：仅识图

直接返回图片中的 Markdown 内容给用户

---

## 注意事项

- iCopy 文件路径含空格，Bash 命令中需用引号包裹或转义
- 若用户消息中同时给出了更具体的图片路径，优先用用户指定的路径
- 图片处理后**不保留本地图片**（OSS 上传后原图可删，仅识图则保留不动）
