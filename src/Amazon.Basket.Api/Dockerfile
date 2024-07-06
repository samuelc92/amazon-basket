FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Amazon.Basket.Api.csproj", "Amazon.Basket.Api/"]
RUN dotnet restore "./Amazon.Basket.Api/./Amazon.Basket.Api.csproj"
COPY . .
WORKDIR "/src/Amazon.Basket.Api"
RUN dotnet build "./Amazon.Basket.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Amazon.Basket.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Amazon.Basket.Api.dll"]