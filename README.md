# CaHaILo: Code Hallucination Illumination

<div align="center">

CoHalLo: code hallucination localization based on hidden layer vector mapping

</div>

---

## 📖 项目简介

CaHaILo 是一个用于检测和分析大型语言模型（LLM）在代码生成和理解任务中产生的幻觉现象的综合性框架。项目采用结构化探针（Structural Probe）技术，通过解码模型内部表示来揭示模型对代码语法结构的理解能力，并提供多维度的幻觉检测。

### 核心特性

- 🔍 **探针分析**：基于神经探针技术解码代码模型的语法理解能力
- 🤖 **多模型支持**：支持 CodeBERT、GraphCodeBERT、CodeT5、UniXcoder 等多种预训练模型
- 🎯 **幻觉检测**：针对主流 LLM（GPT、Claude、DeepSeek、Qwen）的幻觉检测
- 📊 **指标评估**：完整的幻觉类型分类和量化评估体系

---

### 安装步骤

#### 1. 克隆仓库

```bash
git clone https://github.com/student0812/CaHaILo.git
cd CaHaILo
```

#### 2. 创建虚拟环境

```bash
# 使用 conda (推荐)
conda create -n cahailo python=3.11
conda activate cahailo

# 或使用 venv
python -m venv venv
source venv/bin/activate  # Linux/macOS
# venv\Scripts\activate  # Windows
```

#### 3. 安装依赖

```bash
# 安装基础依赖
pip install -r CoHaILo-main/requirements.txt

# 安装 Code-Repair 模块依赖
pip install -r Code-Repair/requirements.txt
```

#### 4. 安装 Tree-sitter

Tree-sitter 用于代码语法解析：

```bash
cd CoHaILo-main/src/resource
mkdir -p grammars
cd grammars

# 下载语言语法库
git clone https://github.com/tree-sitter/tree-sitter-c.git
git clone https://github.com/tree-sitter/tree-sitter-python.git

# 编译语法库
cd ../..
python build_grammars.py
```
---

## 📂 项目结构

```
CaHaILo/
├── CoHaILo-main/                    # 核心探针模型
│   ├── src/
│   │   ├── main.py                  # 主训练脚本
│   │   ├── args.py                  # 参数配置
│   │   ├── line_probe.py            # 探针模型实现
│   │   ├── probe/                   # 探针网络定义
│   │   ├── util/                    # 工具函数
│   │   └── resource/                # 资源文件
│   │       ├── dataset/             # 数据集
│   │       ├── firstStepModels/     # 预训练模型
│   │       └── grammars/            # Tree-sitter 语法
│   ├── run.py                       # 快速运行脚本
│   ├── customize_ast.py             # AST 定制工具
│   ├── do_explain.py                # 解释性分析
│   └── test.py                      # 测试脚本
│
│
├── *_hallucination_detection_updated.py  # LLM 幻觉检测脚本
│   ├── Claude_hallucination_detection_updated.py
│   ├── GPT_hallucination_detection_updated.py
│   ├── deepseek_hallucination_detection_updated.py
│   └── qwen_hallucination_detection_updated.py
│
│
└── 
```
---
## 💻 使用指南

### 1. 数据准备

项目基于 [https://github.com/student0812/CoHaILo/releases/tag/CoHaILo-dataset) 数据集进行评估。
```bash
# 将数据集放置到指定目录
mkdir -p CoHaILo-main/src/resource/dataset/python
# 复制数据集文件到上述目录
# 预处理数据集
cd CoHaILo-main
python src/dataset_generator.py
```

### 2. 训练探针模型

#### 2.1 快速训练（推荐）

```bash
cd CoHaILo-main
python run.py
```

#### 2.2 自定义训练
```bash
python -m src.main \
    --do_train \
    --probe_name my_probe \
    --lang python \
    --first_step_model model_00.bin \
    --first_step_model_type codet5 \
    --rank 128 \
    --lr 1e-4 \
    --epochs 100 \
    --batch_size 32 \
    --seed 42
```
### 3. 测试和评估

#### 3.1 探针模型测试

```bash
cd CoHaILo-main
python test.py \
    --probe_name my_probe \
    --first_step_model_type codet5 \
    --lang python
```

#### 3.2 生成 AST 表示

```bash
python customize_ast.py
```

#### 3.3 运行解释性分析

```bash
python do_explain.py
```

### 4. LLM 幻觉检测

针对不同的 LLM 运行幻觉检测：

#### 4.1 Claude 检测

```bash
python Claude_hallucination_detection_updated.py \
    --input_file tp_fp_filtered_dataset_0.jsonl \
    --output_dir ./results/claude \
    --api_key YOUR_CLAUDE_API_KEY
```

#### 4.2 GPT 检测

```bash
python GPT_hallucination_detection_updated.py \
    --input_file tp_fp_filtered_dataset_0.jsonl \
    --output_dir ./results/gpt \
    --api_key YOUR_OPENAI_API_KEY
```

#### 4.3 DeepSeek 检测

```bash
python deepseek_hallucination_detection_updated.py \
    --input_file tp_fp_filtered_dataset_0.jsonl \
    --output_dir ./results/deepseek \
    --api_key YOUR_DEEPSEEK_API_KEY
```

#### 4.4 Qwen 检测

```bash
python qwen_hallucination_detection_updated.py \
    --input_file tp_fp_filtered_dataset_0.jsonl \
    --output_dir ./results/qwen \
    --api_key YOUR_QWEN_API_KEY
```
---

<div align="center">

**⭐ 如果这个项目对你有帮助，请给我们一个星标！**

Made with ❤️ by the CaHaILo Team

</div>

