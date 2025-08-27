# Docs as Code: Chinese Technical Documentation with Vale Linting

[![Vale](https://img.shields.io/badge/Vale-Lint%20Ready-brightgreen)](https://vale.sh/)
[![AsciiDoc](https://img.shields.io/badge/Format-AsciiDoc-blue)](https://asciidoc.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A comprehensive showcase project demonstrating how to implement **Docs as Code** workflows with Vale static analysis for automated style checking and terminology standardization of Chinese technical documentation.

## 📋 Project Overview

This project demonstrates modern technical documentation management practices, treating documentation as code with version control, automated validation, and continuous integration. Using Vale, we can perform automated checks on Chinese technical documents for:

- ✅ **Terminology Standardization** - Ensure consistent use of technical terms
- ✅ **Safety Language Normalization** - Standardize safety warnings and notices
- ✅ **Format Compliance Checking** - Detect improper use of English abbreviations
- ✅ **Automated Quality Control** - Integrate into CI/CD pipelines for continuous validation

## 🗂️ Project Structure

```
docs-as-code-showcase/
├── README.md                    # Project documentation (this file)
├── example-document.adoc        # Sample: SCADA Formation Machine Operation Manual
├── assets/                      # Image resources directory
│   ├── formation.png           # Formation machine equipment diagram
│   └── scada.jpg              # SCADA monitoring interface screenshot
├── .vale.ini                   # Vale configuration file
├── .vale/styles/               # Vale style rules directory
│   └── ChineseTech/           # Chinese technical documentation style rules
│       ├── meta.json          # Style package metadata
│       ├── Terminology.yml    # Terminology standardization rules
│       ├── SafetyTerms.yml    # Safety terminology rules
│       └── TechnicalFormat.yml # Technical format rules
├── .vscode/settings.json      # VS Code Vale extension configuration
└── CLAUDE.md                  # Detailed project description and Claude usage guide
```

## 🚀 Quick Start

### Prerequisites

1. **Install Vale**
   ```bash
   # Using brew (macOS)
   brew install vale
   
   # Using apt (Ubuntu/Debian)
   sudo apt install vale
   
   # Or download binary directly
   # From https://github.com/errata-ai/vale/releases
   ```

2. **Install AsciiDoctor** (for processing .adoc files)
   ```bash
   # Ubuntu/Debian
   sudo apt install asciidoctor
   
   # macOS
   brew install asciidoctor
   
   # Or using Ruby gem
   gem install asciidoctor
   ```

3. **VS Code Extension** (optional, for real-time checking)
   - Install [Vale VSCode](https://marketplace.visualstudio.com/items?itemName=errata-ai.vale-server) extension

### Usage

1. **Clone the project**
   ```bash
   git clone <repository-url>
   cd docs-as-code-showcase
   ```

2. **Check example documents**
   ```bash
   # Check a single document
   vale example-document.adoc
   
   # Check entire project
   vale .
   
   # Show Vale configuration
   vale ls-config
   ```

3. **View results**
   ```bash
   ✔ 0 errors, 0 warnings and 0 suggestions in 1 file.
   ```

## 📖 Sample Documentation Content

### Document Theme: SCADA Formation Machine Operation Manual

- **Document Format**: AsciiDoc (.adoc)
- **Language**: Simplified Chinese
- **Content Includes**:
  - Equipment overview and safety instructions
  - SCADA interface operation guide
  - Daily maintenance and troubleshooting
  - Technical specifications and parameters
  - Emergency response procedures

### Image Resources

- `formation.png` - Industrial formation machine equipment diagram
- `scada.jpg` - SCADA Human-Machine Interface (HMI) monitoring screenshot

## 🎯 Vale Chinese Style Rules Explained

### 1. Terminology Standardization (Terminology.yml)

**Rule Type**: `substitution` (replacement rules)  
**Level**: `warning`

| Non-standard Term | Standard Term | Description |
|------------------|---------------|-------------|
| 机械 (machinery) | 设备 (equipment) | Standardize on "equipment" for industrial apparatus |
| 装置 (apparatus) | 设备 (equipment) | Standardize equipment terminology |
| 热度 (heat level) | 温度 (temperature) | Use precise technical terminology |
| 气压 (air pressure) | 压力 (pressure) | Distinguish between atmospheric and system pressure |

**Sample Output**:
```
example-document.adoc
 74:8  warning  建议使用技术术语 '温度' 而不是 '热度'  ChineseTech.Terminology
```

### 2. Safety Terminology Standards (SafetyTerms.yml)

**Rule Type**: `substitution` (replacement rules)  
**Level**: `error`

| Informal Term | Standard Term | Importance |
|--------------|---------------|------------|
| 小心 (be careful) | 注意 (attention) | Standardized safety documentation language |
| 当心 (watch out) | 注意 (attention) | Compliance with safety standard requirements |
| 留意 (pay attention) | 注意 (attention) | Formal safety reminder terminology |

**Sample Output**:
```
example-document.adoc
 21:1  error   安全术语建议使用 '注意' 而不是 '小心'  ChineseTech.SafetyTerms
```

### 3. Technical Format Rules (TechnicalFormat.yml)

**Rule Type**: `existence` (existence checking)  
**Level**: `error`

Detects English abbreviations that should not appear in Chinese technical documents:

| English Abbreviation | Recommended Usage | Description |
|---------------------|------------------|-------------|
| Temp: | 温度: (temperature:) | Avoid mixing English and Chinese |
| Press: | 压力: (pressure:) | Maintain Chinese terminology consistency |
| Flow: | 流量: (flow rate:) | Standardize parameter representation |

**Sample Output**:
```
example-document.adoc
 53:8  error   技术文档中应使用规范格式：Temp:  ChineseTech.TechnicalFormat
```

## ⚙️ Configuration Files

### Vale Main Configuration (.vale.ini)

```ini
# Vale configuration for Chinese technical documentation
StylesPath = .vale/styles

# Set minimum alert level (suggestion, warning, error)
MinAlertLevel = suggestion

# Apply Chinese technical style to common documentation formats
[*.{md,adoc,asciidoc,txt}]
BasedOnStyles = ChineseTech
```

### VS Code Integration (.vscode/settings.json)

```json
{
  "vale.valeCLI": {
    "config": ".vale.ini"
  },
  "vale.server": {
    "lintOnChange": true,
    "provideFixes": true
  }
}
```

## 🔄 Docs as Code Workflow

### 1. Writing Phase
- Use AsciiDoc/Markdown to write technical documentation
- Get real-time Vale checking feedback in VS Code
- Follow project terminology and format standards

### 2. Version Control
- Manage documentation with Git just like code
- Every commit triggers automated checks
- Conduct documentation reviews through Pull Requests

### 3. Automated Checking
```bash
# Run in CI/CD pipeline
vale . --output=JSON > vale-report.json
```

### 4. Quality Assurance
- All documentation must pass Vale checks
- Ensure terminology consistency and format compliance
- Automatically generate quality reports

### 5. Publishing and Deployment
- Automatically convert to HTML/PDF formats
- Deploy to documentation websites
- Synchronize releases with product versions

## 🛠️ Advanced Usage

### Custom Rules

Create new Vale rules:

```yaml
# .vale/styles/ChineseTech/CustomRule.yml
extends: substitution
message: "Recommend using '%s' instead of '%s'"
level: warning
swap:
  old-term: new-term
```

### Ignore Specific Content

Add Vale ignore markers in documents:

```asciidoc
<!-- vale off -->
This content will be ignored by Vale checks
<!-- vale on -->
```

### Batch Checking

Check multiple file formats:

```bash
# Check all supported formats
vale --glob='*.{md,adoc,rst,txt}' .

# Check only AsciiDoc files
vale --glob='*.adoc' .
```

## 📊 CI/CD Integration

### GitHub Actions Example

```yaml
name: Documentation Quality Check

on: [push, pull_request]

jobs:
  vale:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Vale Linter
        uses: errata-ai/vale-action@v2
        with:
          styles: |
            https://github.com/chinese-tech-docs/vale-styles/releases/latest/download/ChineseTech.zip
        env:
          GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
```

### GitLab CI Example

```yaml
vale-check:
  stage: test
  image: jdkato/vale:latest
  script:
    - vale --output=JSON . > vale-report.json
  artifacts:
    reports:
      junit: vale-report.json
```

## 🎨 VS Code Experience

After installing the Vale extension:

1. **Real-time Checking** - Shows red underlines while editing
2. **Problems Panel** - Lists all style issues
3. **Quick Fixes** - One-click apply suggested changes
4. **Status Bar** - Shows current document check status

## 🤝 Contributing Guide

### Adding New Rules

1. Create a new `.yml` file under `.vale/styles/ChineseTech/`
2. Define rule type and checking logic
3. Update version information in `meta.json`
4. Add test cases and documentation

### Terminology Suggestions

If you discover new terminology standardization needs:

1. Submit suggestions in Issues
2. Explain the usage context of the terminology
3. Provide standardization rationale
4. Give specific replacement recommendations

## 📚 Related Resources

- [Vale Official Documentation](https://vale.sh/docs/)
- [AsciiDoc Syntax Quick Reference](https://asciidoctor.org/docs/asciidoc-syntax-quick-reference/)
- [Chinese Copywriting Guidelines](https://github.com/chinese-copywriting-guidelines/chinese-copywriting-guidelines)
- [Docs as Code Best Practices](https://www.docsascode.org/)

## 🔍 Troubleshooting

### Common Issues

**Q: Vale cannot find style files**
```bash
# Check configuration path
vale ls-config

# Verify style files exist
ls -la .vale/styles/ChineseTech/
```

**Q: VS Code extension not working**
```bash
# Check if Vale is properly installed
vale --version

# Restart Vale server
Ctrl+Shift+P -> "Vale Server: Restart"
```

**Q: Chinese characters display abnormally**
```bash
# Ensure file encoding is UTF-8
file example-document.adoc
# Should show: UTF-8 Unicode text
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ✨ Acknowledgments

- [Vale](https://vale.sh/) - Excellent documentation style checking tool
- [AsciiDoctor](https://asciidoctor.org/) - Powerful document processing engine
- [Chinese Copywriting Guidelines](https://github.com/chinese-copywriting-guidelines/chinese-copywriting-guidelines) - Chinese copywriting style guide

---

**📝 Last Updated**: $(date +%Y-%m-%d)  
**🤖 Documentation Status**: [![Vale](https://img.shields.io/badge/Vale-Passed-success)](https://vale.sh/)

> By treating documentation as code, we can achieve the same quality standards and automation processes as software development. This project demonstrates how to apply modern development practices to Chinese technical documentation management.