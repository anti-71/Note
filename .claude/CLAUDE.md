# Claudian — Vault 工作指令

> ⚠️ **每次回答前必须先加载核心记忆**
> 执行流程：读 `🧠 核心记忆.md` → 识别任务类型 → 按需加载模块

## 核心记忆加载流程

### Step 1: 读核心（必读）
用 `Read` 读取 `Tools/Claudian Memory/🧠 核心记忆.md`

### Step 2: 判断事务类型，按需加载
根据用户请求的内容，判断事务类型，追加读取对应模块：

| 事务类型 | 用户请求特征 | 额外读取 |
|---------|------------|---------|
| **笔记写作** | 写/改/创建笔记、整理知识 | `📝 笔记规范.md` |
| **识图/OCR** | 识图、转录、图片转文字 | `🖼️ 识图规则.md` + `🤖 工具配置.md` |
| **图床/图片** | 上传图片、OSS、外链 | `🤖 工具配置.md` |
| **Vault 文件** | 结构、目录、导航 | `📂 Vault 结构.md` |
| **通用问答** | 不匹配以上 | 仅 Step 1 |

### Step 3: 配置有变？重新读

### 完整模块索引
```
Tools/Claudian Memory/
├── 🧠 核心记忆.md    ← 先读这里
├── 🤖 工具配置.md    ← OSS + 识图工具
├── 📝 笔记规范.md    ← 写作规范
├── 🖼️ 识图规则.md    ← OCR Prompt
└── 📂 Vault 结构.md  ← 目录布局
```

## 快速参考

- Vault 根：`/Users/jeunesse/Library/Mobile Documents/iCloud~md~obsidian/Documents/Note`
- 路径约定：所有 vault 内操作**使用相对路径**
- 图片处理：**只用 OSS 外链**，不用本地嵌入
- 中文排版：中文与英文/数字/公式间加空格，句尾不加句号
- 笔记风格：YAML frontmatter 用 `名称`/`章`/`节`/`tags`/`index` 字段

> 详细配置见对应模块文件，所有模块索引见 [[Tools/Claudian Memory/🧠 核心记忆.md]]
