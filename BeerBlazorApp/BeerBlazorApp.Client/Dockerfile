FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BeerBlazorApp/BeerBlazorApp.Client/BeerBlazorApp.Client.csproj", "BeerBlazorApp/BeerBlazorApp.Client/"]
RUN dotnet restore "BeerBlazorApp/BeerBlazorApp.Client/BeerBlazorApp.Client.csproj"
COPY . .
WORKDIR "/src/BeerBlazorApp/BeerBlazorApp.Client"
RUN dotnet build "./BeerBlazorApp.Client.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./BeerBlazorApp.Client.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BeerBlazorApp.Client.dll"]
