# 知识库构建

知识库初始化是 Chat2Dashboard 系统的核心功能之一，它允许用户通过上传数据文件（CSV/XLSX）和相关文档，自动创建数据库并生成智能分析代理。系统会自动解析数据结构，生成SQL训练样本，并构建向量知识库以支持自然语言查询。

## 功能特性

- 📊 **多数据源支持**: 
  - 文件上传：支持 CSV、XLSX 文件上传
  - 数据库连接：支持 MySQL、PostgreSQL、SQLite 等主流数据库
- 📚 **文档集成**: 支持 DOCX、PDF、Markdown、TXT 文档上传
- 🤖 **智能代理**: 自动训练数据库查询代理
- 🔍 **向量检索**: 基于多种向量数据库的语义搜索
- 🕸️ **知识图谱**: 自动抽取实体关系并构建知识图谱
- 🧠 **图谱向量化**: 实体和关系的向量化存储与检索
- 📋 **模式管理**: 自动生成和维护数据库模式信息
- 🔄 **增量更新**: 支持模式信息的动态更新
- 🔀 **混合检索**: 结合向量检索、知识图谱检索的多模态检索

## 系统架构流程

```mermaid
graph TB
    subgraph "用户操作层"
        A[用户选择数据源] --> B1[数据库数据]
        A --> B2[文档数据]
        
        B1 --> C1[文件上传方式]
        B1 --> C2[数据库连接方式]
        
        C1 --> D1[上传CSV/XLSX文件]
        C2 --> D2[配置数据库连接]
        D2 --> D2a[MySQL连接配置]
        D2 --> D2b[PostgreSQL连接配置]
        D2 --> D2c[SQLite文件路径]
        D2 --> D2d[其他数据库类型]
        
        B2 --> E[上传文档文件]
        E --> F[DOCX/PDF/MD/TXT文档]
    end

    subgraph "数据处理层"
        D1 --> G1[CSV/XLSX文件解析]
        G1 --> H[数据类型检测]
        H --> I[生成建表SQL]
        I --创建--> J[SQLite数据库]
        subgraph ds["数据库"]
            J
            D2a --> G2[MySQL数据库]
            D2b --> G3[PostgreSQL数据库]
            D2c --> G4[SQLite数据库]
            D2d --> G5[其他数据库]
        end
        
        F --> K[文档存储]
    end

    subgraph "知识库构建层"
        K --> L[构建文档索引]
        ds --> dks
        subgraph dks["数据库知识"]
            M[生成Schema]
            N[SQL样本生成]
        end
        K --> O[文档结构化]
        dks --> O1[数据库知识图谱抽取]

        subgraph "知识图谱抽取"
            O1
            O --> O2[文档知识图谱抽取]
            O1 --> P1[实体识别与抽取]
            O2 --> P1
            P1 --> P2[关系抽取与建模]
            P2 --> P3[图谱实体向量化]
            P3 --> P4[图谱关系向量化]
        end
        dks --> M1["构建数据库索引"]
        P4 --> M2["构建知识图谱索引"]
        L --> Q
        M1 --> Q
        M2 --> Q
        subgraph "构建索引"
            L
            M1
            M2
        end
    end

    subgraph Q["向量数据库"]
        Q1[ChromaDB向量库]
        Q2[Milvus向量库]
        Q3[weaviate向量库]
        Q4[qdrant向量库]
    end
    Q-->R

    subgraph R["检索层"]
        R1[向量检索]
        R2[知识图谱检索]
        R3[混合检索]
    end    
    R --> S
    subgraph S["API服务层"]
        V[RESTful API]
        V --> W[前端界面]
        V --> X[外部集成]
    end

    style A fill:#e1f5fe
    style D1 fill:#f3e5f5
    style D2a fill:#e8f5e8
    style J fill:#f3e5f5
    style P1 fill:#ffe0b2
    style P4 fill:#ffe0b2
    style R fill:#e8f5e8
    style T fill:#fff3e0
    style V fill:#fce4ec
```
