# Fase 1 - Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copia apenas o csproj primeiro para restaurar dependências e melhor aproveitamento do cache do Docker
COPY src/ApiTransacoes/ApiTransacoes.csproj ApiTransacoes/
WORKDIR /src/ApiTransacoes
RUN dotnet restore

# Copia o resto dos arquivos da aplicação
COPY src/ApiTransacoes/. .

# Publica o resultado no diretório /app/publish
RUN dotnet publish -c Release -o /app/publish --no-restore

# Fase 2 - Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

# Copia apenas os arquivos publicados do build
COPY --from=build /app/publish .

# Usa as portas como variável (facilita para alterações posteriores)
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

# Executar com usuário não privilegiado
RUN adduser -h /home/appuser -s /bin/bash -D appuser
USER appuser

ENTRYPOINT ["dotnet", "ApiTransacoes.dll"]
