#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081


# Set a label for the image
LABEL org.opencontainers.image.authors="Barkley Hughes barkley.hughes@csiweb.com"
LABEL org.opencontainers.image.title="Album Artist Weather APP"

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["code-to-cloud-artist-api.csproj", "."]
RUN dotnet restore "./././code-to-cloud-artist-api.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./code-to-cloud-artist-api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./code-to-cloud-artist-api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "code-to-cloud-artist-api.dll"]