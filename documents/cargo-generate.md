# Cargo Generate：快速从模板生成项目的 Rust 利器

在 Rust 生态系统中，`cargo-generate` 是一个非常有用的工具，它可以帮助我们基于现有的模板快速生成新的项目。本文将介绍如何安装和使用 `cargo-generate` 来创建一个新的 Rust 项目。

## 1.安装 `cargo-generate`

首先，我们需要安装 `cargo-generate`。你可以使用以下命令进行安装：

```sh
cargo install cargo-generate
```

## 2.使用 `cargo-generate` 创建项目

在 `cargo-generate`中，创建项目主要有 3 种方式：

- 通过**Git 仓库**创建项目
- 通过**本地文件**创建项目
- 通过**favorite template**创建项目

### 2.1 通过 Git 仓库创建项目

假设我们有一个 GitHub 仓库作为模板，例如 `https://github.com/example/rust-template.git`，我们可以使用以下命令：

```sh
cargo generate --git https://github.com/example/rust-template
# 使用 example/rust-template 默认会到 github 中，去寻找 github.com/example/rust-template 仓库
cargo generate example/rust-template
# 你可以使用博主的模板
cargo generate --git https://github.com/DavidHSiang/rust-simple-template
```

这将会克隆模板仓库，并根据提示创建一个新的项目。

### 2.2 通过本地文件创建项目

如果我们有一个本地的模板文件夹，可以通过指定本地路径来生成项目：

```sh
cargo generate --path /path/to/local/template
```

这种方式非常适合在本地开发和测试模板时使用。

### 2.3 通过 favorite template 创建项目

Cargo Generate 支持将常用的模板定义为“favorite”，方便快速生成项目。你可以在配置文件中定义 favorite 模板，之后通过简单的命令即可使用。

首先，在 `$CARGO_HOME/cargo-generate` 或 `$HOME/.cargo/cargo-generate` 路径下的配置文件中添加常用模板，例如：

```toml
[favorites]
my-template = { git = "https://github.com/example/rust-template.git", branch = "main" }
```

然后，我们可以使用以下命令快速生成项目：

```sh
cargo generate --favorite my-template
```

这种方式可以显著提升创建项目的效率，尤其是在多个项目使用相同模板的情况下。

## 3.自定义模板

创建自己的模板，可以分为以下几个步骤：

1. 创建模板仓库：在 GitHub 或其他平台上创建一个新的仓库，作为模板。
2. 配置模板文件(可选)：在仓库中添加 `.cargo-generate.toml` 文件，定义模板的占位符、钩子等配置。
3. 使用模板：可以通过 `cargo generate` 命令使用模板。

### 3.1 `.cargo-generate.toml` 文件配置

`.cargo-generate.toml` 是 Cargo Generate 用于配置模板行为的文件。它允许你定义占位符、勾子函数等，以更好地控制项目生成过程。

主要包括以下几个部分：

- template: 用于配置模板的基本信息
- placeholders: 用于定义占位符
- hooks: 用于定义勾子函数

下面是一个 `.cargo-generate.toml` 示例文件:

```toml
[template]
name = "example-template"
description = "A simple example template"
version = "0.1.0"

[placeholders]
project-name = { type = "string", prompt = "Project name", default = "my_project", validation = "^[a-zA-Z][a-zA-Z0-9_-]*$" }
author = { type = "string", prompt = "Author name", default = "Anonymous" }
license = { type = "string", prompt = "Choose a license", choices = ["MIT", "Apache-2.0", "GPL-3.0"], default = "MIT" }

[hooks]
pre = ["echo '准备开始生成项目...'"]
post = ["echo '项目已成功生成！'", "git init"]
```

### 3.2 定义占位符

在自定义模板时，我们可以使用占位符来实现模板的动态化。占位符通常使用双花括号包裹，例如 `{{project-name}} `或 `{{author}}`。这些占位符可以在项目生成时根据用户的输入进行替换，从而使项目具有个性化。在模板中，可以将占位符应用于文件名、目录名或者文件内容。

**占位符的使用示例**

在 Cargo.toml 中：

```toml
[package]
name = "{{project-name}}"
version = "0.1.0"
authors = ["{{author}}"]
license = "{{license}}"
```

在创建项目时，Cargo Generate 会自动提示用户为模板中的占位符输入相应的值。我们也可以通过 `.cargo-generate.toml` 文件的 `[placeholders]`部分，为占位符设置默认值和校验规则。

**配置占位符属性**
在 `.cargo-generate.toml` 中，我们可以为占位符设置属性：

- type：占位符的类型，可以是 string、bool 或 number。
- prompt：提示用户输入的消息。
- default：占位符的默认值，当用户没有输入时使用这个值。
- choices：可选值列表，用户可以从中选择一个值。
- validation：用于验证用户输入的正则表达式。

**示例：自定义占位符**

```toml
[placeholders]
include-database = { type = "bool", prompt = "Include database support?", default = true }
```

### 3.3 定义勾子函数

Cargo Generate 支持在模板中定义勾子函数（hook），这些函数可以在项目生成的特定阶段执行自定义命令。常用的勾子函数包括：

- pre：在模板生成前执行命令，例如用于检查环境依赖。
- post：在模板生成后执行命令，例如用于初始化项目配置或运行初始构建。

我们可以通过 `.cargo-generate.toml`文件的 `[hooks]`部分定义勾子函数。例如：

```toml
[hooks]
pre = [
    "echo 'Checking environment...'",
    "rustc --version"
]
post = [
    "echo 'Initializing Git repository...'",
    "git init",
    "git add .",
    "git commit -m 'Initial commit'"
]
```

## 4.总结

```sh
# 安装 cargo-generate
cargo install cargo-generate
# 使用 cargo-generate 生成项目
cargo generate example/rust-template
# 你可以使用博主的模板
cargo generate --git https://github.com/DavidHSiang/rust-simple-template
```

## 5. 参考资源

[Cargo Generate 官方 GitHub](https://github.com/cargo-generate/cargo-generate)
[Cargo Generate 官方文档](https://cargo-generate.github.io/cargo-generate/)
