# CLAUDE.md - AI Assistant Guide for Large Language Models Course

## Project Overview

This is a **Databricks Academy course repository** for teaching Large Language Models (LLMs). The course is available on [edX](https://www.edx.org/course/large-language-models-application-through-production) and covers LLM applications through production deployment.

- **Course Name:** Large Language Models: Application through Production
- **Version:** 2.0.0 (Built: October 19, 2023)
- **Platform:** Databricks (Apache Spark-based)
- **Runtime:** Databricks Runtime 13.3 LTS Machine Learning (CPU or GPU)

## Repository Structure

```
large-language-models/
├── CLAUDE.md                           # This file - AI assistant guidance
├── README.md                           # Course documentation and setup instructions
├── LICENSE.md                          # Databricks proprietary license
├── NOTICE.md                           # Third-party open-source licenses
├── Version Info.py                     # Version metadata and troubleshooting guide
├── Includes/                           # Shared utilities and setup scripts
│   ├── _common.py                      # Core initialization, DBAcademy library setup
│   ├── Classroom-Setup.py              # Environment setup for each notebook
│   ├── Test-Framework.py               # Lab test functions for all lessons
│   ├── Reset.py                        # Environment reset utility
│   ├── Workspace-Setup.py              # Workspace configuration (deprecated)
│   └── Print-Dataset-Copyrights.py     # Dataset copyright information
├── LLM 00 - Introduction/              # Module 0: Course intro, dataset installation
├── LLM 01 - Applications with LLMs/    # Module 1: HuggingFace (summarization, translation)
├── LLM 02 - Embeddings, Vector.../     # Module 2: ChromaDB, embeddings, RAG
├── LLM 03 - Multi-stage Reasoning/     # Module 3: LangChain, agents, chains
├── LLM 04 - Fine-tuning and.../        # Module 4: Model fine-tuning, ROUGE evaluation
├── LLM 05 - Society and LLMs/          # Module 5: Bias detection, ethics, fairness
└── LLM 06 - LLMOps/                    # Module 6: Production deployment
```

## Databricks Notebook Format

All `.py` files in this repository are **Databricks notebooks** with special formatting:

```python
# Databricks notebook source
# MAGIC %md-sandbox
# MAGIC <markdown content>

# COMMAND ----------

# Python code here

# COMMAND ----------
```

### Key Patterns:
- `# Databricks notebook source` - First line identifier
- `# COMMAND ----------` - Cell separator
- `# MAGIC %md` or `# MAGIC %md-sandbox` - Markdown cells
- `# MAGIC %run ./path` - Execute another notebook
- `# MAGIC %pip install` - Install Python packages

## Core Configuration

### Runtime Requirements
From `Includes/_common.py`:
```python
course_config = CourseConfig(
    course_code="llm",
    course_name="large-language-models",
    data_source_version="v03",
    supported_dbrs=["13.3.x-cpu-ml-scala2.12", "13.3.x-gpu-ml-scala2.12"]
)
```

### Cluster Configuration
- **CPU workloads (most modules):** `i3.xlarge` or `i3.2xlarge`
- **GPU workloads (LLM 04 fine-tuning):** `g5.2xlarge` with GPU runtime
- **Cluster type:** Single node (not multi-node)

## Key Dependencies

### External Library
- **DBAcademy v4.0.9** - Core Databricks Academy framework
  - Provides `DBAcademyHelper`, `Paths`, `CourseConfig`, `LessonConfig`
  - Installed automatically via `Includes/_common.py`

### Python Libraries Used
- `transformers` - HuggingFace models and pipelines
- `datasets` - HuggingFace dataset management
- `langchain` - LLM chains, agents, prompt templates
- `chromadb` - Vector database for embeddings
- `torch` - PyTorch deep learning
- `pandas`, `numpy` - Data manipulation
- `rouge` - Text evaluation metrics
- `pinecone`, `weaviate` - Alternative vector DBs (optional modules)

## Development Workflow

### Notebook Initialization Pattern
Every lesson notebook begins with:
```python
# MAGIC %run ../Includes/Classroom-Setup
```

This initializes the `DA` (DBAcademyHelper) object providing:
- `DA.paths.datasets` - Pre-downloaded dataset location
- `DA.paths.working_dir` - User-specific working directory
- Environment validation and configuration

### Data Paths
- **Datasets:** `dbfs:/mnt/dbacademy-datasets/` (shared, read-only)
- **User files:** `dbfs:/mnt/dbacademy-users/` (per-user, writable)
- **Tertiary:** `dbfs:/user/` (Spark default storage)

### API Key Configuration
Notebooks use placeholder patterns for API keys:
```python
os.environ["API_KEY"] = "<FILL IN>"
```

## Testing Framework

### Test Function Pattern
Located in `Includes/Test-Framework.py`:
```python
def dbTestQuestion{LESSON}_{QUESTION}(variables...):
    lesson, question = "lesson{N}", "question{M}"
    userhome_for_testing = getUsernameFromEnv(lesson)

    # Assertions...
    assert condition, "Test NOT passed: message"

    questionPassed(userhome_for_testing, lesson, question)
```

### Test Coverage
| Lesson | Topic | Questions |
|--------|-------|-----------|
| 1 | HuggingFace | 3 (summarization, translation, few-shot) |
| 2 | Embeddings/RAG | 6 (ChromaDB, text generation) |
| 3 | LLM Chains | 4 (retrieval QA) |
| 4 | Fine-tuning | 9 (training, evaluation) |
| 5 | Society/Ethics | 5 (bias, fairness) |

### Test Results
Tests write `PASSED` marker files to:
```
{DA.paths.working_dir}-testing-files/lesson{N}/question{M}/PASSED
```

## Conventions for AI Assistants

### DO:
- Preserve Databricks notebook format (`# MAGIC`, `# COMMAND ----------`)
- Use `DA.paths.*` for file path references
- Follow existing code patterns in each module
- Keep `%run ../Includes/Classroom-Setup` at notebook start
- Maintain compatibility with Databricks Runtime 13.3 LTS ML

### DON'T:
- Remove or modify `# MAGIC` markers
- Change `# COMMAND ----------` cell separators
- Alter the `Includes/` initialization scripts without understanding dependencies
- Assume local filesystem access (use DBFS paths)
- Add dependencies not available in the ML Runtime

### Code Style:
- Notebooks are educational - include explanatory markdown cells
- Variable naming follows Python conventions (snake_case)
- Test functions follow `dbTestQuestion{L}_{Q}` naming pattern
- Error messages should be helpful for learners

## Module Content Summary

| Module | Key Concepts |
|--------|--------------|
| **LLM 00** | Dataset installation, environment setup |
| **LLM 01** | HuggingFace pipelines: summarization, translation, sentiment, few-shot |
| **LLM 02** | Embeddings, ChromaDB vector DB, semantic search, RAG patterns |
| **LLM 03** | LangChain: prompt templates, chains, agents (JekyllHyde, DaScie) |
| **LLM 04** | Model fine-tuning with transformers, ROUGE evaluation metrics |
| **LLM 05** | Bias detection, REGARD scoring, ethics in AI |
| **LLM 06** | LLMOps: production deployment patterns |

## Troubleshooting

Common issues documented in `Version Info.py`:
- **Admin Required:** Some operations need workspace admin privileges
- **Spark Version:** Must use 13.3.x-cpu-ml-scala2.12 or GPU equivalent
- **Cannot Write to DBFS:** Check DBFS mount permissions
- **Cannot Install Libraries:** Network access required for DBAcademy
- **Unity Catalog:** Not required for this course (`requires_uc = False`)

## Quick Reference Commands

```python
# Check environment setup
print(DA.paths.working_dir)
print(DA.paths.datasets)

# Reset environment
%run ../Includes/Reset

# View configuration
print(course_config)
print(lesson_config)
```

## License

This course content is licensed under Databricks proprietary license (see `LICENSE.md`). Third-party dependencies are documented in `NOTICE.md`.
