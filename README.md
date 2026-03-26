# CSAPP Labs (CMU 15-213) - My Solutions & Notes

> A personal repository documenting my journey through the legendary *Computer Systems: A Programmer's Perspective (3rd Edition)* labs. This includes all lab solutions, detailed write-ups, and key learnings.

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
![Progress](https://img.shields.io/badge/Progress-0%20%2F%208%20Labs-orange)

## 📂 项目结构

```text
CSAPP-Labs/
├── README.md                       # 本文件，包含项目说明
├── docs/                           # 实验报告与详细笔记（含解题思路、调试过程）
├── src/                            # 实验源码（可直接编译运行）
├── LICENSE                         # MIT 许可证
└── .gitignore                      # Git 忽略文件配置（如编译产物）
```

> 💡 **提示**：每个实验目录下的源码均已在对应系统中测试通过，笔记包含关键函数分析、测试用例与踩坑记录。

## 📊 实验进度与总结

| 实验 | 名称 | 状态 | 完成日期 | 关键收获/难点 | 实验日记 |
|:---:|:---|:---:|:---:|:---|:---:|
| 1 | src/datalab | ⏳ | - | 位运算的奇妙技巧，理解整数/浮点表示 | - |
| 2 | src/bomblab | ⏳ | - | 汇编代码分析、GDB 实战 | - |
| 3 | src/attacklab | ⏳ | - | 缓冲区溢出攻击原理（代码注入与ROP） | - |
| 4 | src/archlab | ⏳ | - | Y86-64 指令集、流水线 CPU 设计 | - |
| 5 | src/cachelab | ⏳ | - | 缓存模拟器实现、矩阵转置优化 | - |
| 6 | src/shelllab | ⏳ | - | 进程控制、信号处理 | - |
| 7 | src/malloclab | ⏳ | - | 动态内存分配器实现 | - |
| 8 | src/proxylab | ⏳ | - | 网络编程、并发服务器 | - |

*✅ 已完成 🚧 进行中 ⏳ 待开始*

## 🎯 目标与特点
- **完整代码**：包含所有实验的最终通过代码。
- **详细笔记**：每个实验附有**实验日记**，记录解题思路、调试过程和核心知识点。
- **可复现**：提供清晰的构建和运行说明。
- **个人理解**：不仅记录"怎么做"，更记录"为什么"和"学到了什么"。

## 🚀 如何运行
每个实验目录下都有独立的 `README.md` 文件，包含具体的编译和运行指令。通用流程如下：

```bash
# 1. 进入实验目录
cd src/datalab

# 2. 编译
make

# 3. 运行测试
./btest
```

## 📖 实验日记内容概览
在 `docs/` 目录下的每个实验笔记中，你会找到：
- **实验目标**：用一两句话总结本实验要做什么。
- **主要挑战**：记录卡住的地方和如何解决的。
- **关键知识点**：学到的核心系统概念。
- **代码片段与解释**：对关键函数或代码的注释。
- **调试记录**：GDB 命令、调试思路等。
- **总结与思考**：实验带来的启发。

## 📁 文件说明
- **docs/**：包含各实验的详细笔记，格式为 Markdown，便于阅读和分享。
- **src/**：包含各实验的源代码，可直接编译运行，符合 CSAPP 实验要求。
- **LICENSE**：本仓库使用 MIT 许可证，允许自由使用、修改和分发代码，但需保留原许可证声明。

## 🚫 Git 忽略规则
本仓库包含一个 `.gitignore` 文件，用于忽略以下文件：
- 编译产生的二进制文件（如 `*.o`, `*.out`, `*.exe`）
- 可执行文件（如 `bomb`, `ish`）
- 临时文件（如 `*.swp`, `*.swo`）
- 实验框架自带的中间文件
- 个人配置文件

## 📄 许可证
本项目采用 MIT 许可证 - 详情请见 LICENSE 文件。
