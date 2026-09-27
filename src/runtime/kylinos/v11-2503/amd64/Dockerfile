# 变量化 Dockerfile：通过 --build-arg 注入指定 .NET 版本，可生成任意版本镜像
# 例：docker build --build-arg DOTNET_VERSION=9.0.20 -t dotnet-kylinos/runtime:9.0 .
ARG REPO=ghcr.io/blueapple168/dotnet-kylinos/runtime-deps:kylin-v11-2503
ARG DOTNET_VERSION=9.0.20

# Installer image
FROM $REPO AS installer

ARG DOTNET_VERSION

# Retrieve .NET Runtime
RUN dotnet_version=$DOTNET_VERSION \
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

ARG DOTNET_VERSION

# .NET Runtime version
ENV DOTNET_VERSION=$DOTNET_VERSION

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
