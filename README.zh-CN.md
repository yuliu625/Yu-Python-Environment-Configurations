# Python环境配置

## 🎯 核心理念: 配置即代码 (IaC)
本仓库是我用于跨设备管理 Python 开发环境的声明式配置文件集。通过 **“配置即代码”** 的理念，确保不同物理机、服务器与容器之间的开发环境高度一致。

**约定做法: 重建胜于修改 (Rebuild over Patch)**

当依赖变更时，遵循 **“销毁 -> 重建”** 的原则。禁止手动通过命令增量修改环境，所有修改必须在配置文件中实现，以确保环境的**幂等性** 与**纯净度**。


## 🚀 仓库现状: 全面转向现代工具链
> **重要更新：** 本仓库正处于架构迁移阶段。经过深度评估，我决定将环境管理重心从传统的 Conda/Pip 转向以 **uv** 为核心的现代声明式工具链。

### 为什么选择 uv & pixi?
- **极致性能:** 在我理解零拷贝链接和全局缓存机制后，我意识到`uv`未来会成为 Python 社区事实上的管理标准。
- **确定性还原:** 基于 Conda/Pip 难以通过配置文件的方法，即使通过隔离和一次构建，依然无法完全还原环境。而`uv`体系的现代化工具终于彻底解决幂等构建。
- **科学计算兼容:** 因为我目前主要为科研需求的兼容性，我选择使用 **pixi** 对 **Conda** 协议进行支持。我对于其他工具暂时处于观望状态。


## 🛠️ 管理方法与策略
本仓库采用**声明式环境管理**: 每当配置变化，直接销毁并重建环境。(原本的设定如此。)

| 工具 | 状态 | 适用场景 / 优势 |
| --- | --- | --- |
| **uv (首选)** | **Active** | 现代标准，速度极快，支持 `pyproject.toml`，用于个人项目与生产。 |
| **pixi** | **Transition** | 过渡方案，用于处理复杂的科学计算二进制依赖。 |
| **conda** | **Legacy** | 仅为科研任务保留兼容性，**不再主动更新**。 |
| **pip/venv** | **Legacy** | 基础通用方案，**不再主动更新**。 |
| **shell** | **Maintenance** | 用于最彻底的源码编译安装或系统级初始化。 |


## 🏗️ 预构建通用环境
针对不同任务流，我维护了一系列基础配置: 
- **agent_env:** 以 LangChain 和 LangGraph 为核心的智能体开发环境。
- **dl_env:** 深度学习环境，专注于 PyTorch 模型设计与训练。
- **ds_env:** 数据科学研究，包含分析与可视化。
- **llm_env:** LLM运行、部署与训练专用环境。
- **spider_env:** 爬虫开发与自动化工具。
- **web_env:** 后端开发基础环境。

> 更多特定项目配置请查看 `conda_specific_projects/` 目录。


## 如何使用 (Recommended: uv & pixi)
```bash
# 使用 uv 同步项目依赖
uv sync
```

```bash
# 使用 pixi 初始化环境
pixi shell -e dl_env
```


## 如何使用 (Legacy: Conda)
以下为基于`conda`配置的示例，主要原因是conda在科学计算的影响过于久远。但我在未来有计划迁移至`uv`和`pixi`。

### 检查已有环境
首先，检查已经存在的虚拟环境。
```shell
conda env list
```

### 重构环境
如果旧环境存在的话，移除它。
```shell
conda deactivate
conda remove --name ${env_name} --all
```
重建环境。
新环境通过配置文件构建。
```shell
conda env create -f ./environment.yaml
```

### 检查安装的包
最后，检查依赖是否已在环境中。
```shell
conda activate ${env_name}
conda list
```

> 这些命令可以在 `conda_environments/conda_run.sh` 文件中查看。 


## 🛣️ 路线图
- [x] 引入 uv 替换原生 pip 。
- [x] 引入 pixi 解决科研环境的声明式管理。
- [ ] 将所有配置迁移至 pyproject.toml 和 pixi.toml。
- [ ] 基于 Docker Dev Containers 的远程开发集成，彻底解决相关问题的方案。


## 相关项目
我维护的其他工具库，可能对你有用: 
- **[Python-Environment-Configurations](https://github.com/yuliu625/Yu-Python-Environment-Configurations)**: 针对 Python 工具链（Pip, Conda, UV 等）的环境配置仓库。
- **[Deployment-Toolkit](https://github.com/yuliu625/Yu-Deployment-Toolkit)**: 原始 OS 脚本（Shell/PowerShell）的工具包合集。
- **[CLI-Wrapper](https://github.com/yuliu625/Yu-CLI-Wrapper)**: 用 Python 封装的复杂 CLI 工具集，让操作更安全友好。

