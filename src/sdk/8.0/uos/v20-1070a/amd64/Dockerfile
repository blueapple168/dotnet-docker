ARG REPO=ghcr.io/blueapple168/dotnet-uos/aspnet:8.0
# Installer image
FROM $REPO AS installer

# Install .NET SDK
RUN dotnet_sdk_version=8.0.425 \
    && dotnet_version=8.0.31 \
    && curl --fail --show-error --location \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/Sdk/$dotnet_sdk_version/dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/checksums/$dotnet_version-sha.txt \
    && sed -i 's/\r$//' $dotnet_version-sha.txt \
    && sha512sum -c $dotnet_version-sha.txt --ignore-missing \
    && mkdir --parents /dotnet \
    && tar --gzip --extract --no-same-owner --file dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz --directory /dotnet ./packs ./sdk ./sdk-manifests ./templates ./LICENSE.txt ./ThirdPartyNotices.txt \
    && rm \
        dotnet-sdk-$dotnet_sdk_version-linux-x64.tar.gz \
        $dotnet_version-sha.txt

# Download PowerShell
RUN powershell_version=7.4.20 \
    && curl --fail --show-error --location --output PowerShell.Linux.x64.$powershell_version.nupkg https://powershellinfraartifacts-gkhedzdeaghdezhr.z01.azurefd.net/tool/$powershell_version/PowerShell.Linux.x64.$powershell_version.nupkg \
    && powershell_sha256='350ba631e875380dc4dd596777e870a0dddd26c1d2ea5247e84eed020a77e950' \
    && echo "$powershell_sha256  PowerShell.Linux.x64.$powershell_version.nupkg" | sha256sum -c -

# .NET SDK image
FROM $REPO

ENV \
    # Do not generate certificate
    DOTNET_GENERATE_ASPNET_CERTIFICATE=false \
    # Do not show first run text
    DOTNET_NOLOGO=true \
    # SDK version
    DOTNET_SDK_VERSION=8.0.425 \
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
COPY --from=installer ["/PowerShell.Linux.x64.7.4.20.nupkg", "/"]
# Trigger first run experience by running arbitrary cmd
RUN dotnet help

# Install PowerShell global tool
RUN powershell_version=7.4.20 \
    && mkdir --parents /usr/share/powershell \
    && dotnet tool install --add-source / --tool-path /usr/share/powershell --version $powershell_version PowerShell \
    && dotnet nuget locals all --clear \
    && rm -f PowerShell.Linux.x64.$powershell_version.nupkg \
    && ln -s /usr/share/powershell/pwsh /usr/bin/pwsh \
    && chmod 755 /usr/share/powershell/pwsh \
    # Recursively find pwsh binaries and set executable permission (remove hard‑coded alpine path)
    && find /usr/share/powershell/.store -name pwsh -exec chmod 755 {} \; \
    # To reduce image size, remove the copy nupkg that nuget keeps.
    && find /usr/share/powershell -name "*.nupkg" -delete
