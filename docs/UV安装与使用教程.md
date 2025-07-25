# uv 安装与使用教程（Linux 环境）

本教程介绍如何在`Linux`环境下安装和使用教程，详情请参考 [uv 官方文档](https://uv.doczh.com/) 

---

## 1. 在线安装

### 安装

方法一：使用 `curl` 下载脚本并通过 sh 执行：

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

方法二：使用 `wget` 下载脚本并通过 sh 执行：

```bash
wget -qO- https://astral.sh/uv/install.sh | sh
```

## 2. 更新

当 `uv` 通过独立安装程序安装时，它可以按需自我更新：

```bash
uv self update
```

### 3. 卸载

如需从系统中移除 `uv`，请按照以下步骤操作：

清理存储的数据（可选）：

```bash
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"
```

删除 `uv` 和 `uvx` 二进制文件：

```bash
rm ~/.local/bin/uv ~/.local/bin/uvx
```

## 2. 配置国内 PyPI 源

### 2.1 项目级（`pyproject.toml`）

```toml
[[tool.uv.index]]
name = "tsinghua"
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
default = true

[[tool.uv.index]]
name = "aliyun"
url = "https://mirrors.aliyun.com/pypi/simple/"
```

### 2.2 用户级（长期）

在 `~/.bashrc` 中添加：

```bash
# 推荐清华源
echo 'export UV_DEFAULT_INDEX="https://pypi.tuna.tsinghua.edu.cn/simple"' >> ~/.bashrc

# 或者阿里源
# echo 'export UV_DEFAULT_INDEX="https://mirrors.aliyun.com/pypi/simple/"' >> ~/.bashrc

source ~/.bashrc
```

## 3. 离线安装 uv

### 3.1 准备安装包（有网环境）

> `uv` 的发布文件可直接从 [GitHub Releases](https://github.com/astral-sh/uv/releases) 下载。

#### 下载 uv-installer.sh

方法一：使用 `wget` 下载 `install.sh（uv-installer.sh）`脚本

```bash
wget https://astral.sh/uv/install.sh
```

方法二：在网页上输入 <https://astral.sh/uv/install.sh> 即可自动下载 `uv-installer.sh`

#### 修改 `install.h（uv-installer.sh）` 脚本

使用文本编辑器打开`install.h（uv-installer.sh）`文件，找到以下内容（在424行左右）：

```bash
    # download the archive
    local _url="$ARTIFACT_DOWNLOAD_URL/$_artifact_name"
    local _dir
    _dir="$(ensure mktemp -d)" || return 1
    local _file="$_dir/input$_zip_ext"

    say "downloading $APP_NAME $APP_VERSION ${_arch}" 1>&2
    say_verbose "  from $_url" 1>&2
    say_verbose "  to $_file" 1>&2

    ensure mkdir -p "$_dir"

    if ! downloader "$_url" "$_file"; then
      say "failed to download $_url"
      say "this may be a standard network error, but it may also indicate"
      say "that $APP_NAME's release process is not working. When in doubt"
      say "please feel free to open an issue!"
      exit 1
    fi
```

修改为

```bash
    # download the archive
    local _url="$ARTIFACT_DOWNLOAD_URL/$_artifact_name"
    local _dir
    _dir="$(ensure mktemp -d)" || return 1
    local _file="./uv.tar.gz" 
```

使得脚本从当前目录下读取 `./uv.tar.gz` 文件而不是自动从网页 [GitHub Releases](https://github.com/astral-sh/uv/releases) 下载。

#### 下载官方的预编译包

> 如果下载速度慢，可以使用 <https://github.akams.cn/> 加速下载

| 架构 | 下载地址（官方预编译） |
|------|------------------------|
| x86_64 Linux | <https://github.com/astral-sh/uv/releases/latest/download/uv-x86_64-unknown-linux-gnu.tar.gz> |
| aarch64 Linux | <https://github.com/astral-sh/uv/releases/latest/download/uv-aarch64-unknown-linux-gnu.tar.gz> |
| x86_64 macOS | <https://github.com/astral-sh/uv/releases/latest/download/uv-x86_64-apple-darwin.tar.gz> |
| aarch64 macOS | <https://github.com/astral-sh/uv/releases/latest/download/uv-aarch64-apple-darwin.tar.gz> |
| x86_64 Windows | <https://github.com/astral-sh/uv/releases/latest/download/uv-x86_64-pc-windows-msvc.zip> |

把下载的文件重命名为 `uv.tar.gz` 方便脚本处理（在修改`install.h（uv-installer.sh）`时改为当前路径下的读取本地文件 `uv.tar.gz`）。

此时将已经准备好`install.h（uv-installer.sh）`和`./uv.tar.gz`两个文件，放在同一个目录下备用。

### 3.2 准备 Python 解释器

> 前往 [python-build-standalone](https://github.com/astral-sh/python-build-standalone/releases) 可以找到常用的python解释器安装包

使用 `wget` 命令下载对应的 `python` 解释器安装包（此处以 `python3.12` 为例）

```bash
wget https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.12.11%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
```

### 3.3 本地离线安装（无网环境）

> 准备好 `uv-installer.sh`、 `uv.tar.gz` 与 `python` 解释器后执行。

#### 离线安装 uv

将 `uv-installer.sh` 与 `uv.tar.gz` 置于同一目录后执行：

```bash
sh uv-installer.sh
```

安装成功后会打印以下内容

```bash
using local file ./uv.tar.gz
no checksums to verify
installing to /home/ubuntu/.local/bin
  uv
  uvx
everything's installed!
```

验证安装

```bash
uv --version
# 输出示例：uv 0.7.11

uv --help
# 查看可用命令
```

#### 离线安装 python 解释器

将下载好的 `python` 解释器拷贝到无网机器，目录结构示例：

```bash
/root/uv_python_install_mirror/
    └── 20250712/
        └── cpython-3.12.11+20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
```

配置环境变量：将以下内容添加到 `~/.bashrc`，并执行 `source ~/.bashrc` 使其生效：

```bash
export UV_PYTHON_INSTALL_MIRROR=file:///root/uv_python_install_mirror
```

验证安装

```bash
uv python list
# 可以看到我们安装的 Python
```
