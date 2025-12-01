# Use the latest dotnet SDK that supports preview TFMs like net10.0
FROM mcr.microsoft.com/dotnet/nightly/sdk:8.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

# Runtime can be stable 8.0 ASP.NET
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "cartservice.dll"]
