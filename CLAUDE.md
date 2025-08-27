# Docs as Code 中文技术文档 Vale 检查演示

## 项目概述

这是一个 **Docs as Code** 工作流程的展示项目，演示如何使用 Vale 静态分析工具对中文技术文档进行自动化的风格检查和术语标准化。

## 项目结构

```
docs-as-code-showcase/
├── CLAUDE.md                    # 项目说明文档
├── example-document.adoc        # 示例：SCADA系统成型机操作手册 (AsciiDoc格式)
├── assets/                      # 图片资源文件夹
│   ├── formation.png           # 成型机设备结构图
│   └── scada.jpg              # SCADA监控界面截图
├── .vale.ini                   # Vale 配置文件
├── .vale/styles/               # Vale 风格规则目录
│   └── ChineseTech/           # 中文技术文档风格规则
│       ├── meta.json          # 风格包元数据
│       ├── Terminology.yml    # 术语标准化规则
│       ├── SafetyTerms.yml    # 安全术语规则
│       └── TechnicalFormat.yml # 技术格式规则
└── .vscode/settings.json      # VS Code Vale 扩展配置
```

## 演示内容

### 1. 文档内容
- **文档类型**: SCADA系统成型机操作手册
- **文档格式**: AsciiDoc (.adoc)
- **语言**: 简体中文
- **包含内容**: 
  - 设备概述和安全说明
  - SCADA界面操作指南
  - 日常维护和故障排除
  - 技术规格参数

### 2. Vale 中文风格规则

#### 术语标准化 (Terminology.yml)
检查并建议替换不规范的技术术语：
- `机械` → `设备`
- `装置` → `设备` 
- `热度` → `温度`
- `气压` → `压力`

#### 安全术语规范 (SafetyTerms.yml)  
规范安全相关术语的使用：
- `小心` → `注意`
- `当心` → `注意`
- `防护用品` → `个人防护装备`

#### 技术格式规则 (TechnicalFormat.yml)
检查技术文档中的格式规范性：
- 检测英文缩写如 `Temp:` `Press:` `Flow:` 
- 建议使用中文术语 `温度:` `压力:` `流量:`

### 3. 演示效果

当运行 `vale example-document.adoc` 时，Vale 会：
- 识别文档中的术语不规范使用
- 提供具体的替换建议
- 显示错误级别（错误/警告/建议）
- 指出具体的行号和位置

示例输出：
```
example-document.adoc
 21:1  error   安全术语建议使用 '注意' 而不是 '小心'  ChineseTech.SafetyTerms
 74:8  warning 建议使用技术术语 '温度' 而不是 '热度'  ChineseTech.Terminology
```

## 使用方法

### 前提条件
1. 安装 Vale: 参考 Vale 官方文档或使用项目中的可执行文件
2. 安装 AsciiDoctor (用于处理 .adoc 文件): `sudo apt install asciidoctor`
3. VS Code 中安装 Vale 扩展 (可选，用于实时检查)

### 命令行使用
```bash
# 检查单个文档
vale example-document.adoc

# 检查整个项目
vale .

# 显示配置信息
vale ls-config
```

### VS Code 集成
安装 Vale 扩展后，文档中的风格问题会直接在编辑器中显示红色波浪线，并在问题面板中列出具体建议。

## 技术实现

### Vale 配置 (.vale.ini)
```ini
StylesPath = .vale/styles
MinAlertLevel = suggestion

[*.{md,adoc,asciidoc,txt}]
BasedOnStyles = ChineseTech
```

### 自定义规则类型

#### 1. 替换规则 (Substitution)
```yaml
# ChineseTech/Terminology.yml
extends: substitution
message: "建议使用技术术语 '%s' 而不是 '%s'"
level: warning
swap:
  机械: 设备
  热度: 温度
```

#### 2. 存在性检查 (Existence)  
```yaml
# ChineseTech/TechnicalFormat.yml
extends: existence
message: "技术文档中应使用规范格式：%s"
level: error
tokens:
  - 'Temp\s*:'
  - 'Press\s*:'
```

### 规则文件详细说明
所有规则文件都包含详细的英文注释，解释：
- 规则的目的和应用场景
- Vale 规则类型的工作原理
- 每个替换规则的业务逻辑
- 术语标准化的重要性

## 应用场景

这个演示项目适用于：

1. **技术文档团队**: 建立统一的中文技术文档写作标准
2. **本地化团队**: 确保翻译术语的一致性
3. **文档自动化**: 集成到 CI/CD 流程中进行自动化检查
4. **培训演示**: 展示现代化的文档质量控制流程

## Docs as Code 工作流程

1. **编写**: 使用 AsciiDoc/Markdown 编写技术文档
2. **版本控制**: 文档与代码一样使用 Git 管理
3. **自动检查**: Vale 自动检查文档质量和风格规范
4. **持续集成**: 在 PR/MR 中自动运行文档检查
5. **发布**: 自动生成和发布最终文档

通过这种方式，技术文档的质量和一致性得到了有效保障，同时提高了文档维护的效率。

## Claude 使用指南

### 自动生成中文技术文档

当需要生成中文技术文档示例时，Claude 应该：

1. **基于 assets 文件夹图片生成内容**
   - 查看 `assets/formation.png` (成型机设备图) 和 `assets/scada.jpg` (SCADA界面图)
   - 根据图片内容生成对应的技术文档描述
   - 使用 AsciiDoc 格式编写

2. **预置 Vale 检测错误**
   在生成的文档中必须包含以下5类错误，以便 Vale 检测演示：
   - **术语错误**: 使用 "机械" 而不是 "设备"
   - **温度术语**: 使用 "热度" 而不是 "温度" 
   - **压力术语**: 使用 "气压" 而不是 "压力"
   - **安全术语**: 使用 "小心" 而不是 "注意"
   - **设备术语**: 使用 "装置" 而不是 "设备"

3. **文档结构要求**
   ```asciidoc
   = 标题 (基于图片内容)
   :doctype: article
   :toc:
   :imagesdir: assets

   == 概述
   (包含图片引用: image::formation.png[])

   == 操作界面  
   (包含图片引用: image::scada.jpg[])

   == 其他章节
   (包含上述5个Vale错误示例)
   ```

### 使用示例

用户询问：**"生成一个中文技术文档示例"**

Claude 应自动：
1. 分析 assets 文件夹中的图片
2. 生成相应的设备操作手册
3. 在文档中嵌入预定义的术语错误
4. 使用标准 AsciiDoc 格式

这样确保演示时 Vale 能够检测到错误并提供修正建议。