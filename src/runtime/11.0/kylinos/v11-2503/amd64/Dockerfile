ARG REPO=ghcr.io/blueapple168/dotnet-kylinos/runtime-deps:kylin-v11-2503

# Installer image
FROM $REPO AS installer

# Retrieve .NET Runtime
RUN dotnet_version=11.0.0-rc.1.26425.128 \
    && curl --fail --show-error --location \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-x64.tar.gz \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-x64.tar.gz.sha512 \
    && sha512sum -c dotnet-runtime-$dotnet_version-linux-x64.tar.gz.sha512 \
    && mkdir --parents /dotnet \
    && tar --gzip --extract --no-same-owner --file dotnet-runtime-$dotnet_version-linux-x64.tar.gz --directory /dotnet \
    && rm \
        dotnet-runtime-$dotnet_version-linux-x64.tar.gz \
        dotnet-runtime-$dotnet_version-linux-x64.tar.gz.sha512

# .NET runtime image
FROM $REPO AS runtime

# .NET Runtime version
ENV DOTNET_VERSION=11.0.0-rc.1.26425.128

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
