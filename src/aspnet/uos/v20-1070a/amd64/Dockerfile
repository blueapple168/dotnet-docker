# 变量化 Dockerfile：通过 --build-arg 注入指定 ASP.NET Core 版本，可生成任意版本镜像
# 例：docker build --build-arg ASPNET_VERSION=9.0.20 --build-arg REPO=ghcr.io/blueapple168/dotnet-uos/runtime:9.0 -t dotnet-uos/aspnet:9.0 .
ARG REPO=ghcr.io/blueapple168/dotnet-uos/runtime:9.0
ARG ASPNET_VERSION=9.0.20

# Installer image
FROM $REPO AS installer

ARG ASPNET_VERSION

# Retrieve ASP.NET Core
RUN aspnetcore_version=$ASPNET_VERSION \
    && curl --fail --show-error --location \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz \
        --remote-name https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz.sha512 \
    && sha512sum -c aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz.sha512 \
    && mkdir --parents /dotnet \
    && tar --gzip --extract --no-same-owner --file aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz --directory /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm \
        aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz \
        aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz.sha512

# ASP.NET Core image
FROM $REPO AS runtime

ARG ASPNET_VERSION

# ASP.NET Core version
ENV ASPNET_VERSION=$ASPNET_VERSION

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
