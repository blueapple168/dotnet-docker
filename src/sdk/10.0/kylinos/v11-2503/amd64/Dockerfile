ARG REPO=ghcr.io/blueapple168/dotnet-kylinos/aspnet:10.0

# Installer image
FROM $REPO AS installer

# Install .NET SDK
RUN dotnet_sdk_version=10.0.401 \
    && curl --fail --show-error --location \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/Sdk/$dotnet_sdk_version/dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/Sdk/$dotnet_sdk_version/dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz.sha512 \
    && sha512sum -c dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz.sha512 \
    && mkdir --parents /dotnet \
    && tar --gzip --extract --no-same-owner --file dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz --directory /dotnet ./dnx ./packs ./sdk ./sdk-manifests ./templates ./LICENSE.txt ./ThirdPartyNotices.txt \
    && rm \
        dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz \
        dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz.sha512

# Download PowerShell
RUN powershell_version=7.6.6 \
    && curl --fail --show-error --location --output PowerShell.Linux.x64.$powershell_version.nupkg https://powershellinfraartifacts-gkhedzdeaghdezhr.z01.azurefd.net/tool/$powershell_version/PowerShell.Linux.x64.$powershell_version.nupkg \
    && powershell_sha256='4227dacda5fd8817655a0aecd92b7ee923d915a98083fae0e46e31e5c125a0d8' \
    && echo "$powershell_sha256  PowerShell.Linux.x64.$powershell_version.nupkg" | sha256sum -c -

# .NET SDK image
FROM $REPO

ENV \
    # Do not generate certificate
    DOTNET_GENERATE_ASPNET_CERTIFICATE=false \
    # Do not show first run text
    DOTNET_NOLOGO=true \
    # SDK version
    DOTNET_SDK_VERSION=10.0.401 \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip \
    # PowerShell telemetry for docker image usage
    POWERSHELL_DISTRIBUTION_CHANNEL=PSDocker-DotnetSDK-Kylin-V11-2503

RUN set -eux \
    && microdnf install -y \
       git \
       libatomic \
       findutils \
    && microdnf clean all \
    && rm -rf /var/cache/yum/* /var/tmp/* /tmp/* \
    # 修复麒麟基础镜像 Python setuptools 版本问题
    && sed -i 's/68\.0\.0/2168.168.168/g' /usr/lib/python3.11/site-packages/setuptools-68.0.0.dist-info/METADATA

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/PowerShell.Linux.x64.7.6.6.nupkg", "/"]

RUN ln -s /usr/share/dotnet/dnx /usr/bin/dnx \
    # Trigger first run experience by running arbitrary cmd
    && dotnet help

# Install PowerShell global tool
RUN powershell_version=7.6.6 \
    && mkdir --parents /usr/share/powershell \
    && dotnet tool install --add-source / --tool-path /usr/share/powershell --version $powershell_version PowerShell.Linux.x64 \
    && dotnet nuget locals all --clear \
    && rm -f PowerShell.Linux.x64.$powershell_version.nupkg \
    && ln -s /usr/share/powershell/pwsh /usr/bin/pwsh \
    && chmod 755 /usr/share/powershell/pwsh \
    # Recursively find pwsh binaries and set executable permission (avoid hard-coded path)
    && find /usr/share/powershell/.store -name pwsh -exec chmod 755 {} \; \
    # To reduce image size, remove the copy nupkg that nuget keeps.
    && find /usr/share/powershell -name "*.nupkg" -delete
