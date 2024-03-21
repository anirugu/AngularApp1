#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:9.0-preview AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0-preview AS build
RUN bash -E $(curl -fsSL https://deb.nodesource.com/setup_18.x | bash - ); apt install -y nodejs
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["AngularApp1.Server/AngularApp1.Server.csproj", "AngularApp1.Server/"]
RUN dotnet restore "./AngularApp1.Server/AngularApp1.Server.csproj"
COPY . .
WORKDIR "/src/AngularApp1.Server"
RUN dotnet build "./AngularApp1.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./AngularApp1.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AngularApp1.Server.dll"]