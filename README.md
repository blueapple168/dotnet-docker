# .NET
## Featured Repos
* [dotnet/sdk](https://github.com/blueapple168/dotnet-docker/tree/main/src/sdk/): .NET SDK
* [dotnet/aspnet](https://github.com/blueapple168/dotnet-docker/tree/main/src/aspnet/): ASP.NET Core Runtime
* [dotnet/runtime](https://github.com/blueapple168/dotnet-docker/tree/main/src/runtime/): .NET Runtime
* [dotnet/runtime-deps](https://github.com/blueapple168/dotnet-docker/tree/main/src/runtime-deps/): .NET Runtime Dependencies
* [dotnet/monitor](https://github.com/blueapple168/dotnet-docker/tree/main/src/monitor/): .NET Monitor Tool
* [dotnet/aspire-dashboard](https://github.com/blueapple168/dotnet-docker/tree/main/src/aspire-dashboard/): Aspire Dashboard
* [dotnet/samples](https://github.com/blueapple168/dotnet-docker/tree/main/samples/): .NET Samples
## About
[.NET](https://docs.microsoft.com/dotnet/core/) is a general purpose development platform maintained by Microsoft and the .NET community on [GitHub](https://github.com/dotnet/core). It is cross-platform, supports Windows, macOS, and Linux, and can be used in device, cloud, and embedded/IoT scenarios.
.NET has several capabilities that make development productive, including automatic memory management, (runtime) generic types, reflection, [asynchronous constructs](https://learn.microsoft.com/dotnet/csharp/async), concurrency, and native interop. Millions of developers take advantage of these capabilities to efficiently build high-quality applications.
You can use C# or F# to write .NET apps.
* [C#](https://docs.microsoft.com/dotnet/csharp/) is powerful, type-safe, and object-oriented while retaining the expressiveness and elegance of C-style languages. Anyone familiar with C and similar languages will find it straightforward to write in C#.
* [F#](https://docs.microsoft.com/dotnet/fsharp/) is a cross-platform, open-source, functional programming language for .NET. It also includes object-oriented and imperative programming.
[.NET](https://github.com/dotnet/core) is open source (MIT and Apache 2 licenses) and was contributed to the [.NET Foundation](http://dotnetfoundation.org) by Microsoft in 2014. It can be freely adopted by individuals and companies, including for personal, academic or commercial purposes. Multiple companies use .NET as part of apps, tools, new platforms and hosting services.
You are invited to [contribute new features](https://github.com/dotnet/core/blob/main/CONTRIBUTING.md), fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.
> [.NET Documentation](https://docs.microsoft.com/dotnet/core/)
Watch [discussions](https://github.com/blueapple168/dotnet-docker/discussions/categories/announcements) for Docker-related .NET announcements.
## Usage
The [.NET Docker samples](https://github.com/blueapple168/dotnet-docker/blob/main/samples/README.md) show various ways to use .NET and Docker together. See [Introduction to .NET and Docker](https://learn.microsoft.com/dotnet/core/docker/introduction) to learn more.
### Container sample: Run a simple application
You can quickly run a container with a pre-built [.NET Docker image](https://github.com/blueapple168/dotnet-docker/blob/main/README.samples.md), based on the [.NET console sample](https://github.com/blueapple168/dotnet-docker/blob/main/samples/ConsoleApp/README.md).
Type the following command to run a sample console application:
```console
docker run --rm mcr.microsoft.com/dotnet/samples
```
### Container sample: Run a web application
You can quickly run a container with a pre-built [.NET Docker image](https://github.com/blueapple168/dotnet-docker/blob/main/README.samples.md), based on the [ASP.NET Core sample](https://github.com/blueapple168/dotnet-docker/blob/main/samples/AspNetCoreRazorApp/README.md).
Type the following command to run a sample web application:
```console
docker run -it --rm -p 8000:8080 --name aspnetcore_sample mcr.microsoft.com/dotnet/samples:aspnetapp
```
After the application starts, navigate to `http://localhost:8000` in your web browser. You can also view the ASP.NET Core site running in the container from another machine with a local IP address such as `http://192.168.1.18:8000`.
> Note: ASP.NET Core apps (in official images) listen to [port 8080 by default](https://github.com/blueapple168/dotnet-docker/blob/6da64f31944bb16ecde5495b6a53fc170fbe100d/src/runtime-deps/8.0/bookworm-slim/amd64/Dockerfile#L7), starting with .NET 8. The [`-p` argument](https://docs.docker.com/engine/reference/commandline/run/#publish) in these examples maps host port `8000` to container port `8080` (`host:container` mapping). The container will not be accessible without this mapping. ASP.NET Core can be [configured to listen on a different or additional port](https://learn.microsoft.com/aspnet/core/fundamentals/servers/kestrel/endpoints).
See [Hosting ASP.NET Core Images with Docker over HTTPS](https://github.com/blueapple168/dotnet-docker/blob/main/samples/host-aspnetcore-https.md) to use HTTPS with this image.

## 国产系统基础镜像的 .NET 镜像制作与发布

本项目额外支持基于 **统信 UOS** 与 **麒麟 Kylin** 两种国产操作系统基础镜像，构建并发布 .NET 8.0 / 9.0 / 10.0 / 11.0 的 SDK / Runtime / ASP.NET Core 镜像（Runtime-Deps 仅维护 8.0 一份）。

> 说明：`runtime-deps` 层只依赖操作系统、与 .NET 版本无关，因此仅保留 8.0 目录（`src/runtime-deps/8.0/{uos,kylinos}`）；9.0 / 10.0 / 11.0 的 Runtime 构建直接以 `runtime-deps` 系统版本 Tag 为基础镜像。镜像的版本差异由上层 Runtime / ASP.NET / SDK 镜像体现。

### 变量化 Dockerfile（版本控制说明）

Runtime / ASP.NET / SDK 的 Dockerfile 已**变量化**：每种组件每个操作系统只维护一份 Dockerfile，构建时通过 build-args 注入指定 .NET 版本，即可生成任意版本的镜像：

```
src/runtime/uos/v20-1070a/amd64/Dockerfile       # 统信 UOS runtime（变量化）
src/runtime/kylinos/v11-2503/amd64/Dockerfile     # 麒麟 runtime（变量化）
src/aspnet/uos/v20-1070a/amd64/Dockerfile         # 统信 UOS aspnet（变量化）
src/aspnet/kylinos/v11-2503/amd64/Dockerfile      # 麒麟 aspnet（变量化）
src/sdk/uos/v20-1070a/amd64/Dockerfile            # 统信 UOS sdk（变量化）
src/sdk/kylinos/v11-2503/amd64/Dockerfile         # 麒麟 sdk（变量化）
src/runtime-deps/8.0/{uos,kylinos}/...            # 不变量化（与 .NET 版本无关）
```

版本号的**单一事实来源**为 CI workflow（`.github/workflows/docker-build-dotnet.yml`）中的 `env.DOTNET_VERSIONS` 版本表（曾对比过仓库根 `.env`、四组件目录根分散存放两种方案：前者 `docker build` 不原生支持、需额外解析层；后者会拆散 runtime/aspnet/sdk 的版本对应关系，均不如集中定义简单）。新增或修改 .NET 版本只需在该表中增删一行，CI 会自动查表注入 build-args。

各 Dockerfile 的 ARG 均设有默认值（9.0 系列），本地构建可用 `--build-arg` 覆盖生成任意版本镜像：

```console
# 本地构建示例：生成麒麟 10.0 runtime 镜像
docker build \
  --build-arg REPO=ghcr.io/blueapple168/dotnet-kylinos/runtime-deps:kylin-v11-2503 \
  --build-arg DOTNET_VERSION=10.0.12 \
  -t dotnet-kylinos/runtime:10.0 \
  src/runtime/kylinos/v11-2503/amd64
```

### 支持的基础镜像

| 系统 | 基础镜像 | 架构 |
| --- | --- | --- |
| 统信 UOS (v20-1070a) | `registry.uniontech.com/uos-server-base/uos-server-20-1070a:latest` | amd64 |
| 麒麟 Kylin (v11-2503) | `cr.kylinos.cn/kylin/kylin-server-minimal:v11-2503` | amd64 |

### 支持的 .NET 版本

| 系统 | Runtime-Deps | Runtime | ASP.NET | SDK |
| --- | --- | --- | --- | --- |
| 统信 UOS (v20-1070a) | 8.0 | 8.0 / 9.0 / 10.0 / 11.0 | 8.0 / 9.0 / 10.0 / 11.0 | 8.0 / 9.0 / 10.0 / 11.0 |
| 麒麟 Kylin (v11-2503) | 8.0 | 8.0 / 9.0 / 10.0 / 11.0 | 8.0 / 9.0 / 10.0 / 11.0 | 8.0 / 9.0 / 10.0 / 11.0 |

> runtime-deps 仅维护 8.0 一份（该层只依赖操作系统，与 .NET 版本无关），同时发布 `8.0` 与系统版本两类 Tag；9.0 / 10.0 / 11.0 的 Runtime 构建以系统版本 Tag（`uos-20-1070a` / `kylin-v11-2503`）作为基础镜像。

各版本使用的 .NET / PowerShell 版本号（与上游 dotnet-docker 官方镜像保持一致）：

| .NET 版本 | Runtime / ASP.NET Core | SDK | PowerShell |
| --- | --- | --- | --- |
| 8.0 | 8.0.31 | 8.0.425 | 7.4.20 |
| 9.0 | 9.0.20 | 9.0.318 | 7.5.11 |
| 10.0 | 10.0.12 | 10.0.401 | 7.6.6（SDK 新增 `dnx` 目录及 `/usr/bin/dnx`） |
| 11.0 | 11.0.0-rc.1.26425.128 | 11.0.100-rc.1.26425.128 | 7.7.0-preview.2（含 `dnx`） |

> 注意：所有版本的 .NET 二进制包统一使用 `.tar.gz.sha512` 旁车校验文件；PowerShell 工具 ID 8.0 为 `PowerShell`、9.0 起为 `PowerShell.Linux.x64`；PowerShell 校验算法 8.0 ~ 10.0 为 sha256、11.0 为 sha512（以上差异均通过 build-args 变量控制，Dockerfile 中无需区分版本）。10.0 / 11.0 的 SDK 包含 `dnx` 目录，Dockerfile 内自动探测并创建 `/usr/bin/dnx` 符号链接。

### 构建清单（BUILD_LIST / DOTNET_VERSIONS）

构建与发布由 workflow 中的两份清单控制：

**`DOTNET_VERSIONS` 版本表**（`major|runtime版本|sdk版本|PowerShell版本|PowerShell校验算法|PowerShell校验值|PowerShell包ID`）——CI 按主版本号查表，将具体版本号注入变量化 Dockerfile 的 build-args：

```
DOTNET_VERSIONS: |
  8.0|8.0.31|8.0.425|7.4.20|sha256|350ba...e950|PowerShell
  9.0|9.0.20|9.0.318|7.5.11|sha256|926e...7cd4|PowerShell.Linux.x64
  10.0|10.0.12|10.0.401|7.6.6|sha256|4227...a0d8|PowerShell.Linux.x64
  11.0|11.0.0-rc.1.26425.128|11.0.100-rc.1.26425.128|7.7.0-preview.2|sha512|0bfd...d6f|PowerShell.Linux.x64
```

**`BUILD_LIST` 构建清单**，格式为 `源路径;镜像仓库;Tag;系统变体`。runtime/aspnet/sdk 指向变量化 Dockerfile（无版本目录），同一 Dockerfile 按不同 Tag 构建出 4 个版本的镜像；runtime-deps 额外发布系统版本 Tag，与 `8.0` Tag 指向同一镜像：

```
BUILD_LIST: |
  ./src/runtime-deps/8.0/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime-deps;kylin-v11-2503;kylinos
  ./src/runtime-deps/8.0/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime-deps;8.0;kylinos
  ./src/runtime/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime;8.0;kylinos
  ./src/aspnet/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/aspnet;8.0;kylinos
  ./src/sdk/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/sdk;8.0;kylinos
  ./src/runtime/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime;9.0;kylinos
  ./src/aspnet/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/aspnet;9.0;kylinos
  ./src/sdk/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/sdk;9.0;kylinos
  ./src/runtime/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime;10.0;kylinos
  ./src/aspnet/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/aspnet;10.0;kylinos
  ./src/sdk/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/sdk;10.0;kylinos
  ./src/runtime/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime;11.0;kylinos
  ./src/aspnet/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/aspnet;11.0;kylinos
  ./src/sdk/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/sdk;11.0;kylinos
  ./src/runtime-deps/8.0/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime-deps;uos-20-1070a;uos
  ./src/runtime-deps/8.0/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime-deps;8.0;uos
  ./src/runtime/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime;8.0;uos
  ./src/aspnet/uos/v20-1070a/amd64;blueapple168/dotnet-uos/aspnet;8.0;uos
  ./src/sdk/uos/v20-1070a/amd64;blueapple168/dotnet-uos/sdk;8.0;uos
  ./src/runtime/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime;9.0;uos
  ./src/aspnet/uos/v20-1070a/amd64;blueapple168/dotnet-uos/aspnet;9.0;uos
  ./src/sdk/uos/v20-1070a/amd64;blueapple168/dotnet-uos/sdk;9.0;uos
  ./src/runtime/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime;10.0;uos
  ./src/aspnet/uos/v20-1070a/amd64;blueapple168/dotnet-uos/aspnet;10.0;uos
  ./src/sdk/uos/v20-1070a/amd64;blueapple168/dotnet-uos/sdk;10.0;uos
  ./src/runtime/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime;11.0;uos
  ./src/aspnet/uos/v20-1070a/amd64;blueapple168/dotnet-uos/aspnet;11.0;uos
  ./src/sdk/uos/v20-1070a/amd64;blueapple168/dotnet-uos/sdk;11.0;uos
```

  > CI（`.github/workflows/docker-build-dotnet.yml`）按依赖层级串行构建：**优先**构建系统版本 Tag 的 `runtime-deps`（`uos-20-1070a` / `kylin-v11-2503`）→ 其余 `runtime-deps` → `runtime` → `aspnet` → `sdk`，每层内部并行；discovery job 负责变更检测、按 `DOTNET_VERSIONS` 查表注入 build-args；共享构建步骤封装在 `.github/actions/build-image/action.yml`（新增 `build_args` 输入透传给 docker build）。

### Dockerfile 示例

以下为基于国产系统基础镜像构建 .NET 镜像的 Dockerfile 示例（以麒麟 Kylin 的 `runtime-deps` 为例，UOS 同理，仅需更换基础镜像）：

```dockerfile
# syntax=docker/dockerfile:1

# ---- 麒麟 Kylin runtime-deps ----
FROM cr.kylinos.cn/kylin/kylin-server-minimal:v11-2503 AS runtime-deps-kylin

# 安装 .NET 运行所需依赖（按麒麟仓库实际包名调整）
RUN microdnf install -y \
        ca-certificates \
        tzdata \
    && microdnf clean all \
    && rm -rf /var/cache/yum

# ---- 统信 UOS runtime-deps ----
FROM registry.uniontech.com/uos-server-base/uos-server-20-1070a:latest AS runtime-deps-uos

# 安装 .NET 运行所需依赖（按 UOS 仓库实际包名调整）
RUN yum update \
    && yum makecache \
    && yum install -y --setopt=install_weak_deps=false --nogpgcheck --nodocs \
        ca-certificates \
        tzdata \
    && yum clean all \
    && rm -rf /var/cache/yum/*
```

> 提示：具体依赖包、语言环境（`ICU`）与 `ldconfig` 配置需依据目标系统的软件仓库确定；SDK / Runtime / ASP.NET 各层分别在对应的 `runtime-deps` 之上叠加 .NET 二进制包即可。

### 最终发布镜像（ghcr.io/blueapple168）

| 系统 | 镜像 | Tag |
| --- | --- | --- |
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/runtime-deps` | `8.0` / `kylin-v11-2503` |
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/runtime` | `8.0` / `9.0` / `10.0` / `11.0` |
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/aspnet` | `8.0` / `9.0` / `10.0` / `11.0` |
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/sdk` | `8.0` / `9.0` / `10.0` / `11.0` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/runtime-deps` | `8.0` / `uos-20-1070a` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/runtime` | `8.0` / `9.0` / `10.0` / `11.0` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/aspnet` | `8.0` / `9.0` / `10.0` / `11.0` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/sdk` | `8.0` / `9.0` / `10.0` / `11.0` |

> `runtime-deps` 的 `8.0` 与系统版本 Tag（`uos-20-1070a` / `kylin-v11-2503`）指向同一镜像；系统版本 Tag 突出该层只依赖操作系统、与 .NET 版本无关的特性，供 9.0 / 10.0 / 11.0 各层复用。

镜像仓库说明：

```
ghcr.io/blueapple168/dotnet-kylinos/runtime-deps:8.0             # 麒麟 runtime-deps
ghcr.io/blueapple168/dotnet-kylinos/runtime-deps:kylin-v11-2503  # 麒麟 runtime-deps（与 8.0 同一镜像，系统版本 Tag）
ghcr.io/blueapple168/dotnet-kylinos/runtime:8.0                  # 麒麟 runtime
ghcr.io/blueapple168/dotnet-kylinos/runtime:9.0                  # 麒麟 runtime（基于 runtime-deps:kylin-v11-2503）
ghcr.io/blueapple168/dotnet-kylinos/runtime:10.0                 # 麒麟 runtime（基于 runtime-deps:kylin-v11-2503）
ghcr.io/blueapple168/dotnet-kylinos/runtime:11.0                 # 麒麟 runtime（基于 runtime-deps:kylin-v11-2503）
ghcr.io/blueapple168/dotnet-kylinos/aspnet:8.0                   # 麒麟 aspnet
ghcr.io/blueapple168/dotnet-kylinos/sdk:8.0                      # 麒麟 sdk
ghcr.io/blueapple168/dotnet-uos/runtime-deps:8.0                 # 统信 runtime-deps
ghcr.io/blueapple168/dotnet-uos/runtime-deps:uos-20-1070a        # 统信 runtime-deps（与 8.0 同一镜像，系统版本 Tag）
ghcr.io/blueapple168/dotnet-uos/runtime:8.0                      # 统信 runtime
ghcr.io/blueapple168/dotnet-uos/runtime:9.0                      # 统信 runtime（基于 runtime-deps:uos-20-1070a）
ghcr.io/blueapple168/dotnet-uos/runtime:10.0                     # 统信 runtime（基于 runtime-deps:uos-20-1070a）
ghcr.io/blueapple168/dotnet-uos/runtime:11.0                     # 统信 runtime（基于 runtime-deps:uos-20-1070a）
ghcr.io/blueapple168/dotnet-uos/aspnet:8.0                       # 统信 aspnet
ghcr.io/blueapple168/dotnet-uos/sdk:8.0                          # 统信 sdk
```

## Image Variants
.NET container images have several variants that offer different combinations of flexibility and deployment size.
The [Image Variants documentation](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/image-variants.md) contains a summary of the image variants and their use-cases.
### Distroless images
.NET [distroless container images](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/distroless.md) contain only the minimal set of packages .NET needs, with everything else removed.
Due to their limited set of packages, distroless containers have a minimized security attack surface, smaller deployment sizes, and faster start-up time compared to their non-distroless counterparts.
They contain the following features:
* Minimal set of packages required for .NET applications
* Non-root user by default
* No package manager
* No shell
.NET offers distroless images for [Azure Linux](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/azurelinux.md) and [Ubuntu (Chiseled)](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/ubuntu-chiseled.md).
## Related Repositories
.NET:
* [dotnet/nightly/sdk](https://github.com/blueapple168/dotnet-docker/blob/nightly/README.sdk.md): .NET SDK (Preview)
* [dotnet/nightly/aspnet](https://github.com/blueapple168/dotnet-docker/blob/nightly/README.aspnet.md): ASP.NET Core Runtime (Preview)
* [dotnet/nightly/runtime](https://github.com/blueapple168/dotnet-docker/blob/nightly/README.runtime.md): .NET Runtime (Preview)
* [dotnet/nightly/runtime-deps](https://github.com/blueapple168/dotnet-docker/blob/nightly/README.runtime-deps.md): .NET Runtime Dependencies (Preview)
* [dotnet/nightly/monitor](https://github.com/blueapple168/dotnet-docker/blob/nightly/README.monitor.md): .NET Monitor Tool (Preview)
* [dotnet/nightly/aspire-dashboard](https://github.com/blueapple168/dotnet-docker/blob/nightly/README.aspire-dashboard.md): Aspire Dashboard (Preview)
.NET Framework:
* [dotnet/framework](https://github.com/microsoft/dotnet-framework-docker/blob/main/README.md): .NET Framework, ASP.NET and WCF
* [dotnet/framework/samples](https://github.com/microsoft/dotnet-framework-docker/blob/main/README.samples.md): .NET Framework, ASP.NET and WCF Samples
## Support
### Lifecycle
* [Microsoft Support for .NET](https://github.com/dotnet/core/blob/main/support.md)
* [Supported Container Platforms Policy](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/supported-platforms.md)
* [Supported Tags Policy](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/supported-tags.md)
### Image Update Policy
* **Base Image Updates:** Images are re-built within 12 hours of any updates to their base images (e.g. debian:bookworm-slim, windows/nanoserver:ltsc2022, etc.).
* **.NET Releases:** Images are re-built as part of releasing new .NET versions. This includes new major versions, minor versions, and servicing releases.
* **Critical CVEs:** Images are re-built to pick up critical CVE fixes as described by the CVE Update Policy below.
* **Monthly Re-builds:** Images are re-built monthly, typically on the second Tuesday of the month, in order to pick up lower-severity CVE fixes.
* **Out-Of-Band Updates:** Images can sometimes be re-built when out-of-band updates are necessary to address critical issues. If this happens, new fixed version tags will be updated according to the [Fixed version tags documentation](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/supported-tags.md#fixed-version-tags).
#### CVE Update Policy
.NET container images are regularly monitored for the presence of CVEs. A given image will be rebuilt to pick up fixes for a CVE when:
* We detect the image contains a CVE with a [CVSS](https://nvd.nist.gov/vuln-metrics/cvss) score of "Critical"
* **AND** the CVE is in a package that is added in our Dockerfile layers (meaning the CVE is in a package we explicitly install or any transitive dependencies of those packages)
* **AND** there is a CVE fix for the package available in the affected base image's package repository.
Please refer to the [Security Policy](https://github.com/blueapple168/dotnet-docker/blob/main/SECURITY.md) and [Container Vulnerability Workflow](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/vulnerability-reporting.md) for more detail about what to do when a CVE is encountered in a .NET image.
### Feedback
* [File an issue](https://github.com/blueapple168/dotnet-docker/issues/new/choose)
* [Contact Microsoft Support](https://support.microsoft.com/contactus/)
## License
* Legal Notice: [Container License Information](https://aka.ms/mcr/osslegalnotice)
* [.NET license](https://github.com/blueapple168/dotnet-docker/blob/main/LICENSE)
* [Discover licensing for Linux image contents](https://github.com/blueapple168/dotnet-docker/blob/main/documentation/image-artifact-details.md)
* [Windows base image license](https://docs.microsoft.com/virtualization/windowscontainers/images-eula) (only applies to Windows containers)
* [Pricing and licensing for Windows Server](https://www.microsoft.com/cloud-platform/windows-server-pricing)
