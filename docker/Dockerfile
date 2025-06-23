FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY src/ApiTransacoes/ApiTransacoes.csproj ./
RUN dotnet restore ApiTransacoes.csproj
COPY src/ApiTransacoes ./
RUN dotnet publish ApiTransacoes.csproj -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "ApiTransacoes.dll"]

