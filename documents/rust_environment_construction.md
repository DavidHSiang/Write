# Rust CI/CD 环境搭建

Rust 是一种现代系统编程语言，具有高性能和内存安全的特点。本文将介绍如何在你的系统上搭建 Rust 开发环境。

## 1. 安装 Rust

在 Windows 系统中，要下载 [rustup-init.exe](https://static.rust-lang.org/rustup/dist/i686-pc-windows-gnu/rustup-init.exe) 可执行文件进行安装。

在 Linux 或 Macos 系统中，通过如下代码即可安装 Rust：

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

安装完成后，重新启动终端并运行以下命令来验证安装：

```sh
# 查看 rustc 版本
rustc --version
# 查看 cargo 版本
cargo --version
```

## 2. 配置开发环境

### 2.1 设置镜像源（可选）

可以使用国内的镜像源来加速下载。编辑或创建 `~/.cargo/config` 文件，并添加以下内容：

```toml
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "https://mirrors.ustc.edu.cn/crates.io-index"
```

### 2.2 安装常用 CI/CD 工具

#### 2.2.1 安装 cargo generate

`cargo generate` 是一个用于生成项目模板的工具。它可以使用已有的 github repo 作为模版生成新的项目。

- 安装代码如下：

```sh
cargo install cargo-generate
```

- 使用已有的 github repo 作为模版生成新的项目:

```sh
# https://github.com/xxxxx 为 github 仓库的地址
cargo generate --git https://github.com/xxxxx

# 你可以使用博主的模板
cargo generate --git https://github.com/DavidHSiang/rust-simple-template
```

#### 2.2.2 安装 cargo nextest

`cargo nextest` 是一个 Rust 增强测试工具。

- 安装代码如下：

```sh
cargo install cargo-nextest --locked
```

- 对项目进行单元测试：

```sh
cargo nextest run --all-features
```

#### 2.2.3 安装 cargo deny

`cargo deny` 是一个 Cargo 插件，可以用于检查依赖的安全性。

- 安装代码如下：

```sh
cargo install --locked cargo-deny
```

- 在当前项目目录下，生成 cargo deny 的配置文件：

```sh
cargo deny init
```

- 检查依赖的安全性：

```sh
cargo deny check -d
```

#### 2.2.4 安装 typos

`typos` 是一个拼写检查工具，可以监测代码中的拼写错误。

- 安装代码如下：

```sh
cargo install typos-cli
```

安装完成后，在命令行执行命令 `typos`，即可进行拼写检查

可以在当前目录下，通过 `_typos.toml`配置文件，忽略部分文件的拼写检查。

- `_typos.toml`配置文件示例：

```yaml
[default.extend-words]

[files]
extend-exclude = [
    "CHANGELOG.md", # 忽略 CHANGELOG.md 文件
    "notebooks/*", # 忽略 notebooks 目录下的所有文件
]
```

#### 2.2.5 安装 git cliff

`git cliff` 是一个生成 changelog 的工具。

- 安装代码如下：

```sh
cargo install git-cliff
```

#### 2.2.6 安装 pre-commit

`pre-commit` 是一个代码检查工具，可以在提交代码前进行代码检查。

- 安装代码如下：

```sh
pipx install pre-commit
```

- 在 macos 上，也可以使用 brew 进行安装：

```sh
brew install pre-commit
```

- 将 pre-commit 安装至当前项目中：

```sh
pre-commit install
```

**对于每个新项目，若想让 pre-commit 生效，需要将 pre-commit 安装至当前项目中**。当运行 `pre-commit install` 命令后，pre-commit 会在 ` ./.git/hook/` 目录下创建一个 pre-commit 文件。当我们通过 `git commit `提交代码时，会自动根据当前目录下的 `.pre-commit-config.yaml` 配置文件，执行代码检查。

- `.pre-commit-config.yaml` 配置文件示例：

```yaml
fail_fast: false
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.3.0
    hooks:
      - id: check-byte-order-marker
      - id: check-case-conflict
      - id: check-merge-conflict
      - id: check-symlinks
      - id: check-yaml
      - id: end-of-file-fixer
      - id: mixed-line-ending
      - id: trailing-whitespace
  - repo: https://github.com/psf/black
    rev: 22.10.0
    hooks:
      - id: black
  - repo: local
    hooks:
      # 使用 cargo fmt 对代码进行格式化
      - id: cargo-fmt
        name: cargo fmt
        description: Format files with rustfmt.
        entry: bash -c 'cargo fmt -- --check'
        language: rust
        files: \.rs$
        args: []
      # 使用 cargo deny 检查依赖的安全性
      - id: cargo-deny
        name: cargo deny check
        description: Check cargo dependencies
        entry: bash -c 'cargo deny check -d'
        language: rust
        files: \.rs$
        args: []
      # 使用 typos 检查拼写错误
      - id: typos
        name: typos
        description: check typo
        entry: bash -c 'typos'
        language: rust
        files: \.*$
        pass_filenames: false
      # 使用 cargo check 快速检查语法错误
      - id: cargo-check
        name: cargo check
        description: Check the package for errors.
        entry: bash -c 'cargo check --all'
        language: rust
        files: \.rs$
        pass_filenames: false
      # 使用 cargo clippy 分析和检查代码质量、潜在问题以及代码优化建议
      - id: cargo-clippy
        name: cargo clippy
        description: Lint rust sources
        entry: bash -c 'cargo clippy --all-targets --all-features --tests --benches -- -D warnings'
        language: rust
        files: \.rs$
        pass_filenames: false
      # 使用 cargo nextest 进行单元测试
      - id: cargo-test
        name: cargo test
        description: unit test for the project
        entry: bash -c 'cargo nextest run --all-features'
        language: rust
        files: \.rs$
        pass_filenames: false
```

## 3. 创建第一个 Rust 项目

使用 `cargo generate` 创建一个新的 Rust 项目：

```sh
# 创建名为 hello_word 的 rust 项目
cargo generate --git https://github.com/DavidHSiang/rust-simple-template --name hello_word
# 进入项目目录
cd hello_world
# 将 pre-commit 安装至本项目
pre-commit install
```

修改 `Cargo.toml`文件中的项目信息：

```toml
[package]
name = "hello_world"
version = "0.1.0"
edition = "2021"
license = "MIT"

[dependencies]
```

修改 `cliff.toml`文件中的 GitHub 仓库信息（若使用 `DavidHSiang/rust-simple-template`初始化项目需要配置）：

```yaml
# 在 cliff.toml 中，找到如下代码
postprocessors = [
{ pattern = '\$REPO', replace = "https://github.com/XXXX/XXXX" }, # 请修改为自己的 GitHub 仓库
]
```

编写你的第一个 Rust 程序，编辑 `src/main.rs` 文件：

```rust
fn main() {
    println!("Hello, world!");
}
```

## 4. 构建和运行项目

在项目目录中运行以下命令来构建和运行项目：

```sh
cargo build
cargo run
```

你应该会看到输出 `Hello, world!`。至此，Rust CI/CD 环境已经搭建完成了。

## 5.Rust IDE 推荐

### 5.1 VSCode

VSCode 插件推荐：

- crates: Rust 包管理
- Even Better TOML: TOML 文件支持
- Better Comments: 优化注释显示
- Error Lens: 错误提示优化
- GitLens: Git 增强
- Github Copilot: 代码提示
- indent-rainbow: 缩进显示优化
- Prettier - Code formatter: 代码格式化
- REST client: REST API 调试
- rust-analyzer: Rust 语言支持
- Rust Test lens: Rust 测试支持
- Rust Test Explorer: Rust 测试概览
- TODO Highlight: TODO 高亮
- vscode-icons: 图标优化
- YAML: YAML 文件支持

### 5.2 RustRover

RustRover 是 JetBrains 为 Rust 设计的 IDE
