# Stage 1: Build e Publish
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copiar csproj e restaurar dependências
COPY ["Review-Filmes.Web/Review-Filmes.Web.csproj", "Review-Filmes.Web/"]
COPY ["Review-Filmes.Domain/Review-Filmes.Domain.csproj", "Review-Filmes.Domain/"]
RUN dotnet restore "Review-Filmes.Web/Review-Filmes.Web.csproj"

# Copiar código completo
COPY . .

# Publicar a aplicação no caminho fixo /publish
WORKDIR "/src/Review-Filmes.Web"
RUN dotnet publish "Review-Filmes.Web.csproj" -c Release -o /publish /p:UseAppHost=false

# Stage 2: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app

# Copiar arquivos publicados do estágio build
COPY --from=build /publish .

# Garantir permissões para usuário não-root
RUN useradd -m app && chown -R app:app /app
USER app

# Configurações do container
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

ENTRYPOINT ["dotnet", "Review-Filmes.Web.dll"]
