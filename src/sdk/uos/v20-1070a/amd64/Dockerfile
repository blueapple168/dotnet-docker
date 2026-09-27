# 变量化 Dockerfile：通过 --build-arg 注入指定 SDK/PowerShell 版本，可生成任意版本镜像
# 例：
#   docker build \
#     --build-arg REPO=ghcr.io/blueapple168/dotnet-uos/aspnet:9.0 \
#     --build-arg DOTNET_SDK_VERSION=9.0.318 \
#     --build-arg POWERSHELL_VERSION=7.5.11 \
#     --build-arg POWERSHELL_CHECKSUM=926e1abb... \
#     --build-arg POWERSHELL_CHECKSUM_ALGO=sha256 \
#     --build-arg POWERSHELL_PACKAGE_ID=PowerShell.Linux.x64 \
#     -t dotnet-uos/sdk:9.0 .
ARG REPO=ghcr.io/blueapple168/dotnet-uos/aspnet:9.0
ARG DOTNET_SDK_VERSION=9.0.318
ARG POWERSHELL_VERSION=7.5.11
ARG POWERSHELL_CHECKSUM=926e1abb27219b7f21bd1408fc0558d0b8c14eee5f505ca407ed71a1e1457cd4
ARG POWERSHELL_CHECKSUM_ALGO=sha256
ARG POWERSHELL_PACKAGE_ID=PowerShell.Linux.x64

# Installer image
FROM $REPO AS installer

ARG DOTNET_SDK_VERSION
ARG POWERSHELL_VERSION
ARG POWERSHELL_CHECKSUM
ARG POWERSHELL_CHECKSUM_ALGO

# Install .NET SDK
RUN dotnet_sdk_version=$DOTNET_SDK_VERSION \
    && curl --fail --show-error --location \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/Sdk/$dotnet_sdk_version/dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/Sdk/$dotnet_sdk_version/dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz.sha512 \
    && sha512sum -c dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz.sha512 \
    && mkdir --parents /dotnet \
    # 10.0+ SDK 包含 ./dnx 目录，自动探测后追加到解压列表，8.0/9.0 无此目录自动跳过
    && extract_list="./packs ./sdk ./sdk-manifests ./templates ./LICENSE.txt ./ThirdPartyNotices.txt" \
    && if tar --list --file dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz | grep -q '^ \{0,1\}\./dnx/'; then \
        extract_list="./dnx $extract_list"; \
    fi \
    && tar --gzip --extract --no-same-owner --file dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz --directory /dotnet $extract_list \
    && rm \
        dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz \
        dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz.sha512

# Download PowerShell
RUN powershell_version=$POWERSHELL_VERSION \
    && powershell_checksum=$POWERSHELL_CHECKSUM \
    && powershell_algo=$POWERSHELL_CHECKSUM_ALGO \
    && curl --fail --show-error --location --output PowerShell.Linux.x64.$powershell_version.nupkg https://powershellinfraartifacts-gkhedzdeaghdezhr.z01.azurefd.net/tool/$powershell_version/PowerShell.Linux.x64.$powershell_version.nupkg \
    && echo "$powershell_checksum  PowerShell.Linux.x64.$powershell_version.nupkg" | ${powershell_algo}sum -c -

# .NET SDK image
FROM $REPO

ARG DOTNET_SDK_VERSION
ARG POWERSHELL_VERSION
ARG POWERSHELL_PACKAGE_ID

ENV \
    # Do not generate certificate
    DOTNET_GENERATE_ASPNET_CERTIFICATE=false \
    # Do not show first run text
    DOTNET_NOLOGO=true \
    # SDK version
    DOTNET_SDK_VERSION=$DOTNET_SDK_VERSION \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip \
    # PowerShell telemetry for docker image usage
    POWERSHELL_DISTRIBUTION_CHANNEL=PSDocker-DotnetSDK-UOS-V20

RUN set -eux \
    && yum install -y --setopt=install_weak_deps=false --nogpgcheck --nodocs \
       git \
       libatomic \
       findutils \
    && yum clean all \
    && rm -rf /var/cache/yum/* /var/tmp/* /tmp/*

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/PowerShell.Linux.x64.*.nupkg", "/"]

RUN set -eux \
    # 10.0+ SDK 提供 dnx 工具，存在时创建符号链接
    && if [ -e /usr/share/dotnet/dnx ]; then ln -s /usr/share/dotnet/dnx /usr/bin/dnx; fi \
    # Trigger first run experience by running arbitrary cmd
    && dotnet help

# Install PowerShell global tool
RUN powershell_version=$POWERSHELL_VERSION \
    && powershell_package_id=$POWERSHELL_PACKAGE_ID \
    && mkdir --parents /usr/share/powershell \
    && dotnet tool install --add-source / --tool-path /usr/share/powershell --version $powershell_version $powershell_package_id \
    && dotnet nuget locals all --clear \
    && rm -f PowerShell.Linux.x64.$powershell_version.nupkg \
    && ln -s /usr/share/powershell/pwsh /usr/bin/pwsh \
    && chmod 755 /usr/share/powershell/pwsh \
    # Recursively find pwsh binaries and set executable permission (avoid hard-coded path)
    && find /usr/share/powershell/.store -name pwsh -exec chmod 755 {} \; \
    # To reduce image size, remove the copy nupkg that nuget keeps.
    && find /usr/share/powershell -name "*.nupkg" -delete
