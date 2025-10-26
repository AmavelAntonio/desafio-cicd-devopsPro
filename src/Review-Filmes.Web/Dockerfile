# Stage 1: Base para runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

# Garantir permissões corretas para o usuário 'app'
RUN mkdir -p /app && chown -R app:app /app

# Define o usuário não root
USER app

# Expor porta da aplicação
EXPOSE 8080

# Variáveis de ambiente úteis para logging e container
ENV DOTNET_RUNNING_IN_CONTAINER=true
ENV DOTNET_LOGGING__CONSOLE__ENABLECOLORS=true
ENV ASPNETCORE_URLS=http://+:8080

# Stage 2: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copiar projetos individualmente para aproveitar cache do Docker
COPY ["Review-Filmes.Web/Review-Filmes.Web.csproj", "Review-Filmes.Web/"]
COPY ["Review-Filmes.Domain/Review-Filmes.Domain.csproj", "Review-Filmes.Domain/"]

# Restaurar dependências
RUN dotnet restore "./Review-Filmes.Web/Review-Filmes.Web.csproj"

# Copiar restante do código
COPY . .

WORKDIR "/src/Review-Filmes.Web"

# Build
RUN dotnet build "./Review-Filmes.Web.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Stage 3: Publish
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Review-Filmes.Web.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Stage 4: Final
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Entry point para rodar a aplicação continuamente
ENTRYPOINT ["dotnet", "Review-Filmes.Web.dll"]
