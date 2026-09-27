ARG REPO=ghcr.io/blueapple168/dotnet-uos/runtime:10.0

# Installer image
FROM $REPO AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=10.0.12 \
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

# ASP.NET Core version
ENV ASPNET_VERSION=10.0.12

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
