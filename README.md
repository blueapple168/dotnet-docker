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

本项目额外支持基于 **统信 UOS** 与 **麒麟 Kylin** 两种国产操作系统基础镜像，构建并发布 .NET 8.0 的 SDK / Runtime / ASP.NET Core / Runtime-Deps 镜像。

### 支持的基础镜像

| 系统 | 基础镜像 | 架构 |
| --- | --- | --- |
| 统信 UOS (v20-1070a) | `registry.uniontech.com/uos-server-base/uos-server-20-1070a:latest` | amd64 |
| 麒麟 Kylin (v11-2503) | `cr.kylinos.cn/kylin/kylin-server-minimal:v11-2503` | amd64 |

### 构建清单（BUILD_LIST / Dockerfile 配置）

构建与发布清单如下，格式为 `源路径;镜像仓库;Tag;系统变体`：

```
BUILD_LIST: |
  ./src/aspnet/8.0/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/aspnet;8.0;kylinos
  ./src/runtime-deps/8.0/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime-deps;8.0;kylinos
  ./src/runtime/8.0/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/runtime;8.0;kylinos
  ./src/sdk/8.0/kylinos/v11-2503/amd64;blueapple168/dotnet-kylinos/sdk;8.0;kylinos
  ./src/aspnet/8.0/uos/v20-1070a/amd64;blueapple168/dotnet-uos/aspnet;8.0;uos
  ./src/runtime-deps/8.0/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime-deps;8.0;uos
  ./src/runtime/8.0/uos/v20-1070a/amd64;blueapple168/dotnet-uos/runtime;8.0;uos
  ./src/sdk/8.0/uos/v20-1070a/amd64;blueapple168/dotnet-uos/sdk;8.0;uos
```

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
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/runtime-deps` | `8.0` |
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/runtime` | `8.0` |
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/aspnet` | `8.0` |
| 麒麟 Kylin | `ghcr.io/blueapple168/dotnet-kylinos/sdk` | `8.0` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/runtime-deps` | `8.0` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/runtime` | `8.0` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/aspnet` | `8.0` |
| 统信 UOS | `ghcr.io/blueapple168/dotnet-uos/sdk` | `8.0` |

镜像仓库说明：

```
ghcr.io/blueapple168/dotnet-kylinos/runtime-deps:8.0   # 麒麟 runtime-deps（示例）
ghcr.io/blueapple168/dotnet-kylinos/runtime:8.0        # 麒麟 runtime
ghcr.io/blueapple168/dotnet-kylinos/aspnet:8.0         # 麒麟 aspnet
ghcr.io/blueapple168/dotnet-kylinos/sdk:8.0            # 麒麟 sdk
ghcr.io/blueapple168/dotnet-uos/runtime-deps:8.0       # 统信 runtime-deps
ghcr.io/blueapple168/dotnet-uos/runtime:8.0            # 统信 runtime
ghcr.io/blueapple168/dotnet-uos/aspnet:8.0             # 统信 aspnet
ghcr.io/blueapple168/dotnet-uos/sdk:8.0                # 统信 sdk
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
