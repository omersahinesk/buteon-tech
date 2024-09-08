FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 44000
EXPOSE 44001

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Rufinus.Auth.Http.Api/Rufinus.Auth.Http.Api.csproj", "Rufinus.Auth.Http.Api/"]
RUN dotnet restore "Rufinus.Auth.Http.Api/Rufinus.Auth.Http.Api.csproj"
COPY . .
WORKDIR "/src/Rufinus.Auth.Http.Api"
RUN dotnet build "Rufinus.Auth.Http.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Rufinus.Auth.Http.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Rufinus.Auth.Http.Api.dll"]
