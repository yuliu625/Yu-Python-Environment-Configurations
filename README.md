# Python Environment Configurations

## 🎯 Core Philosophy: Infrastructure as Code (IaC)

This repository is a collection of declarative configuration files used to manage my Python development environments across multiple devices. Adopting the **"Infrastructure as Code"** philosophy, I ensure high consistency across physical machines, remote servers, and containers.

**Guiding Principle: Rebuild over Patch**

When dependencies change, I follow the **"Destroy -> Rebuild"** principle. Manual incremental modifications via CLI are discouraged. All changes must be implemented within configuration files to guarantee **idempotency** and **environment purity**.

## 🚀 Status: Transitioning to Modern Toolchains

> **Important Update:** This repository is currently undergoing an architectural migration. Following a deep evaluation, I have decided to shift the focus of environment management from traditional Conda/Pip to a modern, declarative toolchain centered around **uv**.

### Why uv & pixi?

- **Extreme Performance:** Having understood zero-copy linking and global caching mechanisms, I believe `uv` will become the de facto management standard for the Python community.
- **Deterministic Reproduction:** Conda and Pip struggle to achieve perfect reproduction via config files alone. The `uv` ecosystem finally solves idempotent builds through modern lockfile mechanisms.
- **Scientific Computing Compatibility:** To maintain compatibility with research requirements, I use **pixi** for Conda-protocol support. Other tools remain under observation.

### Why Miniforge?

> **Licensing Note:** Due to changes in Anaconda's commercial terms, this repository has completely phased out Anaconda/Miniconda distributions in favor of **[Miniforge](https://github.com/conda-forge/miniforge)**.

- **Miniforge Advantages:** It uses the `conda-forge` channel by default, avoiding commercial licensing restrictions, and offers superior support for ARM architectures.
- **Channel Strategy:** The `defaults` channel is forcibly removed to ensure all dependencies are maintained by the open-source community via `conda-forge`.


## 🛠️ Management Strategy

This repository employs **Declarative Environment Management**: whenever a configuration changes, the environment is destroyed and rebuilt from scratch.

| Tool | Status | Use Case / Advantages |
| --- | --- | --- |
| **uv (Preferred)** | **Active** | Modern standard, blazing fast, supports `pyproject.toml`; used for personal projects and production. |
| **pixi** | **Transition** | Bridge solution for handling complex scientific computing binary dependencies. |
| **conda (Miniforge)** | **Legacy** | Retained for research task compatibility; **no longer actively updated**. |
| **pip/venv** | **Legacy** | Basic universal solution; **no longer actively updated**. |
| **shell** | **Maintenance** | Used for full source-code compilation or system-level initialization. |


## 🏗️ Pre-configured General Environments

I maintain a series of base configurations tailored for different workflows:

- **agent_env:** Agent development environment centered on LangChain and LangGraph.
- **dl_env:** Deep Learning environment, focused on PyTorch model design and training.
- **ds_env:** Data Science research, including analysis and visualization.
- **llm_env:** Dedicated environment for LLM execution, deployment, and training.
- **spider_env:** Web crawling and automation tools.
- **web_env:** Base environment for backend development.

> For project-specific configurations, please refer to the `conda_specific_projects/` directory.


## Usage (Recommended: uv & pixi)

```bash
# Sync project dependencies using uv
uv sync
```

```bash
# Initialize environment using pixi
pixi shell -e dl_env
```

## Usage (Legacy: Miniforge)

As Conda remains deeply rooted in the scientific community, some legacy projects still retain `.yaml` configurations. **Note: Ensure you are using [Miniforge](https://github.com/conda-forge/miniforge).**

### Disabling the `defaults` Channel (Security Setup)

To avoid Anaconda's licensing constraints and embrace the open-source community, disable `defaults` in your global config:

```shell
# Disable defaults and set strict priority
conda config --set auto_update_conda False
conda config --remove channels defaults || true
conda config --add channels conda-forge
conda config --set channel_priority strict
```

### Configuration Standard

All `environment.yaml` files in this repo have been updated to force `conda-forge` and disable the `defaults` channel.

```yaml
# Example snippet
channels:
  - conda-forge
  - nodefaults  # Explicitly forbid defaults
dependencies:
  ...
```

## Usage (Legacy: Conda)

The following are examples based on `conda`. While Conda has a long-standing influence in scientific computing, I plan to migrate these to `uv` and `pixi` in the future.

### Check Existing Environments

First, list the existing virtual environments.

```shell
conda env list
```

### Reconstruct Environment

If an old environment exists, remove it.

```shell
conda deactivate
conda remove --name ${env_name} --all
```

Rebuild the environment using the configuration file.

```shell
conda env create -f ./environment.yaml
```

### Verify Installation

Finally, verify that the dependencies are correctly installed.

```shell
conda activate ${env_name}
conda list
```

> These commands are also documented in the `conda_environments/conda_run.sh` file.


## 🛣️ Roadmap

- [x] Migrate to Miniforge (Deprecate Anaconda/Miniconda and `defaults` channel).
- [x] Introduce `uv` to replace native `pip`.
- [x] Introduce `pixi` for declarative management of research environments.
- [ ] Migrate all configurations to `pyproject.toml` and `pixi.toml`.
- [ ] Integrate Remote Development based on Docker Dev Containers for a "total solution."


## Related Projects

Other toolkits I maintain that you might find useful:

- **[Python-Environment-Configurations](https://github.com/yuliu625/Yu-Python-Environment-Configurations)**: Environment configurations for Python toolchains (Pip, Conda, UV, etc.).
- **[Deployment-Toolkit](https://github.com/yuliu625/Yu-Deployment-Toolkit)**: A collection of raw OS scripts (Shell/PowerShell).
- **[CLI-Wrapper](https://github.com/yuliu625/Yu-CLI-Wrapper)**: A safer, more user-friendly Python wrapper for complex CLI tools.

